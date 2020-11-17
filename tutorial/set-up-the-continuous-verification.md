# Set up the continuous analysis

## Add a tis.config file

{% hint style="info" %}
To set up the continuous analysis, we need to add the project to TrustInSoft CI and create a tis.config file, so TrustInSoft CI knows how to analyze our project. Let's start with the tis.config file.
{% endhint %}

### Create a tis.config file

**1.** Once on your own demo-caesar project page on GitHub, check that you are on branch **master** and click **Create new file**:

![](../.gitbook/assets/image%20%28128%29.png)

**2.** Name the file **tis.config**:

![](../.gitbook/assets/image%20%2855%29.png)

### Add a first test configuration object to tis.config

{% hint style="info" %}
A test configuration object can be defined as a sort of "light" ****specification in JSON of a given test in your project test suite.

For each specified test, TrustInSoft CI will emulate a user-defined hardware architecture and propagate the test's input values, statement by statement, from the beginning until the test's main function returns or an undefined behavior has been detected.
{% endhint %}

In the demo-caesar repository, the source file [main.c](https://github.com/TrustInSoft-CI/demo-caesar/blob/master/main.c) includes a test, which encrypts and decrypts the string "People of Earth, your attention please", using 2 different shift values -3 and 7.

Let's specify this test in a configuration object by providing both the source files used by this test and the compilation options, that'd be used if we were to compile this test.

Then, let's embed it inside a JSON array, so more tests can be added later:

```text
[
    {
        "name": "Test shift values 7 and -3",
        "files": [ "main.c", "caesar.c" ],
        "compilation_cmd": "-I ."
    }
]
```

**1.** Copy-paste the above configuration object to your tis.config file:

![](../.gitbook/assets/image%20%28176%29.png)

**2.** Finally, push the tis.config file on your master branch by clicking **Commit new file**:

![](../.gitbook/assets/image%20%287%29.png)

Now that you've created your first tis.config file, feel free to visit [Writing tis.config file](../reference/configuration-file.md) to learn about all the available configuration options.

## Trigger the first analysis

### Sign up to TrustInSoft CI

**1.** Visit TrustInSoft CI [Sign-in page](https://ci.trust-in-soft.com/signin) and click **Sign in/up with GitHub**.

**2.** If not already signed in, sign in to GitHub.

You'll be redirected to the GitHub authorization page for TrustInSoft CI to be granted access to your GitHub projects:

![](../.gitbook/assets/image%20%285%29.png)

{% hint style="info" %}
Note: TrustInSoft CI requests both Read & Write access to your **public** **repositories** but TrustInSoft CI will **never** write to them. Requiring Write access on top of Read is a known limitation of the [GitHub API](https://developer.github.com/apps/building-oauth-apps/authorizing-oauth-apps/).
{% endhint %}

**3.** Optionally grant access to any of your [GitHub organizations](../reference/github-organizations.md), that own C and C++ projects, by clicking **Grant**.

**4.** Click **Authorize TrustInSoft**.

You'll be redirected to the Welcome to TrustIn Soft CI/Add project page.

### **Trigger the first analysis in Add project**

{% hint style="info" %}
The Welcome to TrustIn Soft CI/Add project page lists all the steps to set up the continuous analysis of your project. Moving forward, it can also serve as your cheatsheet since you can always come back to it in the navigation bar.
{% endhint %}

**1.** In **Select your project**, select the project **demo-caesar**:

![](../.gitbook/assets/image%20%28123%29.png)

**2.** Skip the middle section, since you've already added a tis.config file.

**3.** Finally in the last section, click **Start verifying**:

![](../.gitbook/assets/image%20%28158%29.png)

This will trigger a first analysis on branch **master** and display the ongoing analysis page.

### **Observe your first undefined behavior**

**1.** Wait for the analysis to complete, then look at the results in **Verification status** \(see the capture below\).

**Verification Status** displays the analyzed tests counts according to four categories with a specific color for each, such as red for _Undefined behavior_ and green for _No undefined behavior_. In our case, only one test has been configured and it is red so an undefined behavior has been found.

**2.** Click on the only test. The detected undefined behavior corresponds to an invalid memory access:

![](../.gitbook/assets/image%20%28194%29.png)

Now that we know that the project demo-caesar contains an undefined behavior, let's understand its root cause!

