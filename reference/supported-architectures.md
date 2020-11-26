# Supported architectures

## Introduction

During the analysis, TrustInSoft CI faithfully emulates a target hardware architecture, respecting properties such as endianness, integer types and alignment constraints.

For each test, you can specify the target architecture of your choice by setting the `machdep` field in the tis.config file to the corresponding architecture name. If `machdep` is not provided, TrustInSoft CI emulates, by default, a 32-bit x86 processor.

See below the list of currently supported architectures or [read more about machdep](configuration-file.md#machdep).

## Supported architectures <a id="list"></a>

{% hint style="info" %}
With the exception of`apple_ppc_32, x86_win32` and`x86_win64`, all target architectures may be specified with the `gcc_` prefix, in which case gcc language extensions are allowed, and the`__int128`integer type is available on a 64-bit`machdep`.
{% endhint %}

Download a more detailed description of each supported architecture:

{% file src="../.gitbook/assets/trustinsoft-ci-supported-architectures-2020-11-25.csv" caption="Supported architectures" %}

| Architecture name in machdep | Endianness | Description |
| :--- | :---: | :--- |
| `x86_16` | Little | An x86 processor in 16-bit mode, in “tiny” or “small” memory model \(using 16-bit “near” pointers\). The **long double** type is the 80387 80-bit native format. |
| `x86_16_huge` | Little | An x86 processor in 16-bit mode, in “large” or “huge” memory model \(using 32-bit “far” pointers\). The **long double** type is the 80387 80-bit native format. |
| `x86_32` | Little | An x86 processor in 32-bit mode. The **long double** type is the 80387 80-bit native format. |
| `x86_win32` | Little | An x86 processor in 32-bit mode, using the Win32 ABI. |
| `x86_64` | Little | An x86-64 processor in 64-bit mode. The **long double** type is the IEEE754 quad-precision type. |
| `x86_win64` | Little | An x86 processor in 64-bit mode, using the Win64 ABI. The **long** type is 32 bits. |
| `ppc_32` | Big | A 32-bit PowerPC processor. The **char** type is unsigned; the **long double** type is the IEEE754 quad-precision type. |
| `ppc_64` | Big | A 64-bit PowerPC processor. The **char** type is unsigned; the **long double** type is the IEEE754 quad-precision type. |
| `apple_ppc_32` | Big | Same as `ppc_32`, but forcing the “char” type to be signed, as done by the Apple toolchain \(as opposed to the default PowerPC use of unsigned char type\), and allowing gcc language extensions. |
| `arm_eabi` | Little | A 32-bit ARM processor, using the extended ABI \(EABI\). The **char** type is unsigned. |
| `armeb_eabi` | Big | A 32-bit ARM processor, using the extended ABI \(EABI\). The **char** type is unsigned. |
| `aarch64` | Little | A 64-bit ARMv8 processor \(AArch64\). The **char** type is unsigned; the **long double** type is the IEEE754 quad-precision type. |
| `aarch64eb` | Big | A 64-bit ARMv8 processor \(AArch64\). The **char** type is unsigned; the **long double** type is the IEEE754 quad-precision type. |
| `sparc_32` | Big | A 32-bit SparcV8 processor. |
| `sparc_64` | Big | AA 64-bit SparcV9 processor. The **long double** type is the IEEE754 quad-precision type. |
| `mips_o32` | Big | A 32-bit MIPS processor, using the old ABI \(o32\). |
| `mipsel_o32` | Little | A 32-bit MIPS processor, using the old ABI \(o32\). |
| `mips_n32` | Big | A 64-bit MIPS processor, using the new 32-bit ABI \(n32\). The **long double** type is the IEEE754 quad-precision type. |
| `mipsel_n32` | Little | A 64-bit MIPS processor, using the new 32-bit ABI \(n32\). The **long double** type is the IEEE754 quad-precision type. |
| `mips_64` | Big | A 64-bit MIPS processor, using the 64-bit ABI \(64 or n64\). The **long double** type is the IEEE754 quad-precision type. |
| `mipsel_64` | Little | A 64-bit MIPS processor, using the 64-bit ABI \(64 or n64\). The **long double** type is the IEEE754 quad-precision type. |
| `rv32ifdq` | Little | A RISC-V processor using the RV32I ISA, with D, F and Q floating-point extensions. The **long double** type is the IEEE754 quad-precision type. |
| `rv64ifdq` | Little | A RISC-V processor using the RV64I ISA, with D, F and Q floating-point extensions. The **long double** type is the IEEE754 quad-precision type. |

## Fundamental data types

Unless otherwise specified in the above list, the characteristics of the fundamental data types are:

| Data type | Description |
| :--- | :--- |
| **char** | is 8 bits, and defaults to signed |
| **short** | is 16 bits |
| **int** | is 16 bits on 16-bit `machdeps` \(`x86_16`, `x86_16_huge`\), 32 bits otherwise |
| **long** | is 64 bits on 64-bit `machdeps`, 32 bits otherwise |
| **long long** | is 64 bits |
| **float** | is 32 bits \(IEEE754 single-precision type\) |
| **double** | is 64 bits \(IEEE754 double-precision type\) |
| **long double** | is identical to **double** |

