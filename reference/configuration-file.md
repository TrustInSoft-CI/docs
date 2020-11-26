# Writing a tis.config file

## Introduction to tis.config

For TrustInSoft CI to know how to analyze your project, you need to specify at least one test configuration object in a tis.config file, place the tis.config file in your project root directory and push it to the branch\(es\) that you'd like to analyze.

### One configuration object

A test configuration object is a light specification in JSON of a given test in your project's test suite. For each specified test, TrustInSoft CI will detect all the undefined behaviors along the test path.

Here is an example of a tis.config file, taken from our [tutorial](../tutorial/), with one configuration object:

```text
[    
    {
        "name": "Test shift values 7 and -3",
        "files": [ "main.c", "caesar.c" ],
        "cpp-extra-args": "-I ."
    }
]
```

A typical test configuration object may contain the following fields \(most are optional\):

* `name`: the name you give to the test / test configuration object
* `files`: an exhaustive list of all the source files \(such as `.c`, `.cpp` ...\) used by the test
* `cpp-extra-args`:  the compilation options to tell TrustInSoft CI how to parse the source files before the analysis \(use `cxx-cpp-extra-args`for C++ files\)
* `main`: the name of the test's entry point function
* `machdep`: the target hardware architecture for the analysis

See [Fields in tis.config](configuration-file.md#fields-in-tis-config) for more details or to get a list of all the fields you can use.

### Multiple configuration objects

Generally, you will start with specifying one test in the tis.config file so one configuration object. As you certainly have more than one test in your test suite, you will add more test configuration objects to the tis.config file.

Here is an example of a tis.config file with two configuration objects:

```text
[
  {
      "name": "Test shift values 7 and -3",
      "files": [ "main.c", "caesar.c" ],
      "cpp-extra-args": "-I ."
  },
  {
      "name": "Test long input string",
      "files": [ "tests/long_string.c", "caesar.c" ],
      "cpp-extra-args": "-I. -Itests"
  }
]
```

### Adding comments

The tis.config file should be written using the JSON [ECMA-404 standard](https://www.ecma-international.org/publications/standards/Ecma-404.htm), with a syntax extension for comments:

* `//` ignores all characters until the end of line
* `/*` ignores all characters until the next `*/`

## Fields in the tis.config file <a id="fields"></a>

### `address-alignment`

* **Description:** The base addresses are assumed to be aligned to multiples of n. ****
* **Type**: integer 
* **Default:** `1` 
* **Example:** `"address-alignment": 65536`

### `compilation_cmd`

{% hint style="warning" %}
This field is now deprecated. Use[`cpp-extra-args`](configuration-file.md#cpp-extra-args)or[`cxx-cpp-extra-args`](configuration-file.md#cxx-cpp-extra-args)intead.
{% endhint %}

### `compilation-database`

{% hint style="info" %}
This field is mostly relevant if you are using tools such as [CMake](https://cmake.org/) or [Bear](https://github.com/rizsotto/Bear). In any case, the generated compilation database file\(s\) should be available in the GitHub repo.
{% endhint %}

* **Description**: Specifies one or a set of [compilation database JSON file\(s\)](https://clang.llvm.org/docs/JSONCompilationDatabase.html) to tell TrustInSoft CI how to parse the source files prior to the analysis. `compilation-database` can either replace or be used as a complement to `cpp-extra-args`. If used as complement, TrustInSoft CI will first fetch the compilation options in `compilation-database`, then append the ones in `cpp-extra-args`.  `compilation-database` accepts the following values: 
  * `true`: to enable reading the compilation database and scan all the 'compile\_commands.json' files located anywhere in the GitHub repository 
  * filename or list of filenames:
    * If the filename is a directory, TrustInSoft CI will scan every ‘compile\_commands.json’ file in this directory and its sub-directories.
    * Otherwise, it will scan the given filename.
    * A list of filenames, where for every filename, the above process is applied. 
* **Type**:
  * boolean
  * string
  * list of strings

### `cpp-extra-args`

* **Description**: The [preprocessing](https://en.wikipedia.org/wiki/C_preprocessor) options that are relevant to your project and the analysis \(such as `-I`, `-D` or `-U` \) or the whole compilation line, to let TrustInSoft CI know how to parse the C source files prior to the analysis. TrustInSoft CI will keep only the relevant options.  For C++ files, use [`cxx-cpp-extra-args`](configuration-file.md#cxx-cpp-extra-args) instead. If your project contains source files in both languages, include both fields in the tis.config file. 
* **Type**: string 
* **Example:** `"cpp-extra-args": "-DFOO -Itool"`

### `cxx-cpp-extra-args`

* **Description**: The [preprocessing](https://en.wikipedia.org/wiki/C_preprocessor) options that are relevant to your project and the analysis \(such as `-I`, `-D` or `-U` \) or the whole compilation line, to let TrustInSoft CI know how to parse the C++ source files prior to the analysis. TrustInSoft CI will keep only the relevant options.  For C files, use [`cpp-extra-args`](configuration-file.md#cpp-extra-args) instead. If your project contains source files in both languages, include both fields in the tis.config file. 
* **Type**: string 
* **Example:** `"cxx-cpp-extra-args": "-DFOO -Itool"`

### `cxx-std`

* **Description**: Specifies the C++ standard to follow. 
* **Type**: string 
* **Default:** `“c++11"`

### `files`

* **Description**: The list of all the source files \(such as `.c` , `.cpp` ...\) used by the test. If all of them are contained in the project's root directory, `“all”` may be used instead to include all the sources files in the root directory. 
* **Type**:

  * `“all”`
  * list of strings

* **Required**: yes

### `filesystem`

* **Description**: Is required as soon as your code needs to read a file from the GitHub repository \(such as a test vector\). In that case, `filesystem` must contain both the full file name in the GitHub repository and the exact expected name in the source code. If some of it is missing, the analysis may stop with a **Bad libc call** error. 
* **Type**: object with a single field: 
  * `files`: The list of files in the GitHub repository that your code needs to read from.

    _Type:_ list of objects with fields:  


    * `name` _\(required\):_ Full name of the file \(or directory\) as expected by the source code. The name should exactly match the string given to the filesystem function\(s\). _Type:_ string 
    * `from`: Full name of the file \(or directory\) in the GitHub repository. Omitting this field acts as if the file does not exist. _Type:_ string 
* **Example:**

```text
"filesystem": { 
    "files": [ 
        {
            "name": "/usr/share/input_file.txt",
            "from": "main_input_file.txt"
        }
    ] 
}
```

### `include`

* You may use the option `include` to load other configuration files, which must contain only a single test configuration each.

  ```text
  { "include": "path/to/another_configuration_file.config" }
  ```

  For each option in the included configuration:  


  * If the option has not been declared _before_ the special option `include`, it will be **added** to the main configuration. 
  * If the option has already been declared _before_ the special option `include`:
    * If the option type is a list of values, the value of the option in the included configuration will be **concatenated** to the existing one in the main configuration.
    * Otherwise, the value of the option in the included configuration will **replace** the value in the main configuration. 

  For any option declared _after_ the `include` option in the main configuration, the value of the option will either be added, be concatenated or will replace the one declared in the included configuration by following the same rules as above.  


  The following two test configurations:

  ```text
  {
    "files": [ "test.c" ],
    "machdep": "gcc_x86_32",
    "include": "dir/other.config",
    "name": "My first test"
  }
  ```

  ```text
  {
    "files": [ "other.c" ],
    "machdep": "gcc_x86_64",
    "name": "The generic part"
  }
  ```

  are equivalent to this single test configuration:

  ```text
  {
    "files": [ "test.c", "dir/other.c" ],
    "machdep": "gcc_x86_64",
    "name": "My first test"
  }
  ```

  **Including several configurations files**  


  Several configuration files can be included with a single `include` option.

  ```text
  {
    "include":
      [
        "path/to/sub_config1.config",
        "path/to/sub_config2.config",
        "common/common.config"
      ]
  }
  ```

  In this case, the first configuration file to include is processed according to the above rules. Then the resulting configuration will be considered as the main one for the second configuration file to include, and so on...

### `machdep`

* **Description**: The target hardware architecture for the analysis. 
* **Type**: string among the following values \(see [Supported architectures](supported-architectures.md#supported-architectures) for definitions\): 
  * `“aarch64”`
  * `“aarch64eb”`
  * `“apple_ppc_32”`
  * `“arm_eabi”`
  * `“armeb_eabi”`
  * `“gcc_aarch64”`
  * `“gcc_aarch64eb”`
  * `“gcc_arm_eabi”`
  * `“gcc_armeb_eabi”`
  * `“gcc_mips_64”`
  * `“gcc_mips_n32”`
  * `“gcc_mips_o32”`
  * `“gcc_mipsel_64”`
  * `“gcc_mipsel_n32”`
  * `“gcc_mipsel_o32”`
  * `“gcc_ppc_32”`
  * `“gcc_ppc_64”`
  * `“gcc_rv32ifdq”`
  * `“gcc_rv64ifdq”`
  * `“gcc_sparc_32”`
  * `“gcc_sparc_64”`
  * `“gcc_x86_16”`
  * `“gcc_x86_16_huge”`
  * `“gcc_x86_32”`
  * `“gcc_x86_64”`
  * `“mips_64”`
  * `“mips_n32”`
  * `“mips_o32”`
  * `“mipsel_64”`
  * `“mipsel_n32”`
  * `“mipsel_o32”`
  * `“ppc_32”`
  * `“ppc_64”`
  * `“rv32ifdq”`
  * `“rv64ifdq”`
  * `“sparc_32”`
  * `“sparc_64”`
  * `“x86_16”`
  * `“x86_16_huge”`
  * `“x86_32”`
  * `“x86_win32”`
  * `“x86_64”`
  * `“x86_win64”` ****
* **Default**: `“gcc_x86_32”`

### `main`

* **Description**: The function name which serves as entry point for the test to verify. 
* **Type**: string 
* **Default:** `“main”`

### `name`

* **Description**: An optional name that you give to your test / test configuration in TrustInSoft CI. 
* **Type**: string

### `no-results`

* **Description:** Add this line `"no-results": true` to the tis.config file in order to make the tests run faster and consume less memory. Warning: the values of the variables will no longer be available in the Analyzer view. ****
* **Type**: boolean 
* **Default:** `false`

### `prefix_path`

* **Description:** A path that will be used as prefix for all file names provided in the configuration file. ****
* **Type**: string 
* **Example:** `"prefix_path": "src/dir"`

### `val-args`

* **Description:** Enables to pass arguments to the test's entry point function. The first character is used as separator to split the arguments, a space works well in the common cases. 
* **Type**: string 
* **Example:** `"val-args": " input1.gz input2.gz"`

### `val-timeout`

* **Description:** Enables to override the default timeout value per test. 
* **Type**: integer between `60` and `10800` \(3 hours\) 
* **Default:** `900` \(15 minutes\)  Note: If a value greater than `10800` is set, the test will still be stopped at 3 hours.

