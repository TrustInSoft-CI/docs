# Changelog

## 2021-06-01

- [x] **New feature: Dashboard**

Previously, you had to use your browser history or bookmark to go back to a visited project that you do not own. And if you have a lot of projects, you may get lost in the project lists.

This is the past. Now, a new Dashboard is available to focus on your recent activity, including your latest updated projects and the latest projects you visited.

Some shortcut links are also available at the top of the Dashboard. Let us know which action you use the most to include it as a shortcut in this page in the future.

- [x] **New feature: Search**

Do you want to find a particular project or user in TrustInSoft CI?

The new Search input is available at the top-right of the page to allow you to find and browse any project or user using TrustInSoft CI.

As a bonus, if you click in this input without writing anything, the latest project you visited will be shown. It allows to switch between projects more efficiently without going through the Dashboard case!

- [x] **New feature: Explore**

Never two without three.

You are missing inspiration? You want to look at how other users are using TrustInSoft CI? Do you want to know more about the fine details of the C standards? Or just browsing the Internet during the coffee break?

The new Explore page is for you! This page will display various things such as:
- the most popular projects and users of TrustInSoft CI (but the popularity algorithm is kept secret!)
- the latest blog posts of our C/C++ experts
- some hints about our other services if TrustInSoft CI do not cover all your needs


- [x] **New feature: Project settings**

A new settings page is available for each project (only for projects you owned). Settings related to this project will be shown on this page.

Also, you can now allow builds to be automatically canceled if a newer one is in the queue for the same reference (branch/tag/Pull Request).

- [x] **New feature: Support for Pull Requests**

If you read the earlier point, you have already been spoiled.

Indeed, now GitHub Pull Requests are supported, and a build is automatically launched if the Pull Request has a `tis.config` file (like any regular branch or tag).

- [x] **New designs**

- With all these new features, the navigation bar has been entirely re-designed, to keep a quick access to any useful feature.

- We also care about small devices. So, we improved the responsive design, allowing results to be easily read from smartphones and tablets too.

- The contrast of some color has also been increased in both the application and the email notifications. It should now be easier to spot problem at a glance.

- [x] **Inspecting analyses results**

The "Explore" button to inspect in detail the analyses result of a test has been renamed into "Inspect" (to not be mistaken with the new "Explore" feature).

Also, the "Inspect" feature has also been disabled for tests using the `"no-result": true` field. With this field, nothing relevant can be inspected within the `Analyzer` view.

- [x] And of course, a lot of various invisible **improvements and bug fixes** have been done to make your visit on TrustInSoft CI as comfortable as possible.


## 2021-01-12

* [x] **The Analyzer view is now open to guest users!** Previously, guest users had no choice but to create a TrustInSoft CI account to browse analysis results with the Analyzer. As TrustInSoft CI user, this made it impractical when you wanted to question someone, who'd never used TrustInSoft CI, about an Undefined Behavior.  Now, you can click on an Explore button, copy-paste the Analyzer URL in the address bar and share it with anyone, who can then freely look at the Undefined Behavior, without ever having to create an account:

![](.gitbook/assets/image%20%28190%29.png)

* [x] **Pull requests and git tags are now supported.** From now on, a new analysis will be triggered every time a pull request is created / updated or a git tag is pushed to your remote repository. Also learn if any given analysis refers to a branch update, a pull request or a tag by reading the REFERENCE columns which replaces the BRANCH column. 
* [x] Other small improvements and fixes

## 2020-11-26

* [x] Some users have asked us to share a **detailed description of each supported target architecture** so they can pick the right subset of architectures to run their analyses on and therefore save analysis time.  It's done! The csv file available [here](reference/supported-architectures.md#fundamental-data-types) contains, for each supported architecture, information such as `char` size, `short` size, `int` size, etc. 
* [x] **New fields** [**`cpp-extra-args`**](reference/configuration-file.md#cpp-extra-args) **and** [**`cxx-cpp-extra-args`**](reference/configuration-file.md#cxx-cpp-extra-args) **in the tis.config file.** Use one or both of these two fields to let TrustInSoft CI know how to [preprocess](https://en.wikipedia.org/wiki/C_preprocessor) the source files in your project prior to the analysis.  You can provide either the preprocessing options that are relevant to your project and the analysis \(such as `-I`, `-D` or `-U` \) or the whole compilation line. TrustInSoft CI will keep only the relevant options.  Use `cpp-extra-args` and `cxx-cpp-extra-args` for respectively C and C++ source files. If your project contains source files in both languages, include both fields in the tis.config file.

{% hint style="warning" %}
These fields replace and deprecate the[`compilation_cmd`](reference/configuration-file.md#compilation_cmd)field. Please replace it with`cpp-extra-args`and/or`cxx-cpp-extra-args`in all your tis.config file!
{% endhint %}

## 2020-11-10

* [x] **Tired of working on 2 different tabs?** **The** [**Analyzer**](tutorial/find-the-root-cause-of-the-undefined-behaviors.md#launch-trustinsoft-ci-analyzer) **now opens in the same browser tab!** Previously, after you clicked on the Explore button, a new tab opened to load the Analyzer. When by chance the opening of this tab wasn't already blocked by your browser, this behavior was painful, since you had to go back and forth between the main TrustInSoft CI tab and this Analyzer tab. 
* [x] **You can now share Analyzer URLs with other logged-in users!** And in an upcoming release, share them with logged-out users too! For example, it can be quite handy when you want to question someone about an Undefined Behavior found. Click on an Explore button, copy-paste the Analyzer URL in the address bar and share it with others:

![](.gitbook/assets/image%20%28190%29.png)

* [x] **New field** [**`no-results`**](reference/configuration-file.md#no-results) **in the tis.config file.** Some of your tests are taking too long or are running into an out-of-memory? Feel free to add this line `"no-results": true` to your tis.config file. The tests will run faster and consume less memory.

{% hint style="warning" %}
Warning: the values of the variables will no longer be available in the Analyzer view.
{% endhint %}

* [x] **New field** [**`cxx-std`**](reference/configuration-file.md#cxx-std) **in the tis.config file.** Use this option to specify the C++ standard to follow. Default is `c++11`. ****
* [x] **2 new target architectures are supported:** `x86_win32` and `x86_win64`.[ Learn more](reference/supported-architectures.md#list)  
* [x] **New column "EXIT STATUS" in the Tests table.** By providing details on how your test terminated, this new column enables you to quickly spot the tests that prematurely exited or aborted. Hover your mouse over the Question mark icon to learn about the possible values.

![](.gitbook/assets/image%20%2856%29.png)

{% hint style="warning" %}
When`"no-results"`is set in the [tis.config](reference/configuration-file.md) file, values in this column may be missing in some cases. We are working on it.
{% endhint %}

* [x] Other small improvements and fixes

## 2020-09-03

* [x] Start receiving email notifications every time a verification is done! Several email trigger options are available. [Learn more](reference/receiving-email-notifications.md) 
* [x] The field `raw_options` is now deprecated: please remove all of its occurrences from your [tis.config](reference/configuration-file.md) files. Here is an example of tis.config file before and after the requested change:

{% tabs %}
{% tab title="tis.config before change" %}
```yaml
{
    ...
    "raw_options": {"-val-timeout": "%d"}
    ...
}
```
{% endtab %}

{% tab title="tis.config after change" %}
```yaml
{
    ...
    "val-timeout": %d
    ...
}
```
{% endtab %}
{% endtabs %}

* [x] Other small improvements and fixes

## 2020-07-28

* [x] **New test status:** **Invalid property.** You may for example use this new status to verify the result of your tests. Annotate your code with an [ACSL property](https://frama-c.com/acsl_tutorial_index.html). If TrustInSoft CI fails to verify such property, the test will fail with this status.  In the below example, TrustInSoft CI will check if the `vector_test` function returns `0` and it will return the **Invalid property** status otherwise:

```c
//@ ensures \result == 0;
int vector_test(void (*f)(c​onst vector[], vector*),
                        const char *name, size_t nb_inputs,
                        size_t nb_vectors, u8 **vectors, size_t *sizes)
```

* [x] Various small improvements and fixes especially on test statuses, filter and pagination

## 2020-07-06

* [x] Add your first [TrustInSoft CI status badge](reference/status-badge.md) to your project's README!
* [x] See 20 tests at a time with the new pagination component
* [x] Use the new test status filter in order, for example, to focus on the tests with an undefined behavior
* [x] You can now override the default 15-minute timeout value per test by adding the new option [`val-timeout`](reference/configuration-file.md#val-timeout) to your [tis.config](reference/configuration-file.md) file as follows:`"val-timeout": <v>`, where `<v>` is the new timeout value per test in seconds between 60 and 10800 \(3 hours\) 
* [x] Other small improvements and bug fixes

## 2020-06-05

* [x] You can now view up to 1000 tests or verifications on the same page versus 50 before
* [x] The memory allocated to verify each test has been doubled from 1.7GB to 3.5GB
* [x] For a given verification, you may now see, next to the Verification Result field, either a Warning icon for you to take action \(add or fix your tis.config file\) or a Cross icon to inform you that the Verification Result is either missing or incomplete \(Git error or canceled verification\)
* [x] Other small improvements and bug fixes

## 2020-05-25

* [x] **More granular Test Status**: when your test has encountered an error, you do not need to click the test anymore to know the exact error type, just read the more granular Test Status field
* [x] **More detailed Verification Status**: the Verification Status field now displays the detailed tests results instead of a consolidated result. As a result, the Tests Summary column has been removed \(as redundant\)
* [x] The field [`name`](https://docs.ci.trust-in-soft.com/reference/configuration-file#name) in the tis.config file becomes optional: TrustInSoft CI will set a default name to your test when [`name`](https://docs.ci.trust-in-soft.com/reference/configuration-file#name) is not set
* [x] The field [`files`](https://docs.ci.trust-in-soft.com/reference/configuration-file#files) is no longer required in every tis.config file, where the field [`include`](https://docs.ci.trust-in-soft.com/reference/configuration-file#include) is used
* [x] New Delete button on the Verification history/project page: feel free to delete select verifications within your project history to share a clean TrustInSoft CI verification history with others
* [x] The analysis of programs, that contain floating-point values, is now more precise and can handle infinities and NaNs
* [x] Other small improvements and bug fixes

## 2020-04-20

* [x] C++ is now officially supported: check out our [C++ tutorial](tutorial-cxx/) and start analyzing C++ projects!
* [x] Easy configuration: error messages related to missing or invalid [tis.config](reference/configuration-file.md) files are more precise
* [x] The undefined behavior \`Overflow\` becomes \`Signed overflow\`
* [x] TrustInSoft CI Analyzer re-loads much faster: from 50s down to 10s for a 5s analysis \(except the first time the Analyzer is loaded\)
* [x] Other small improvements and bug fixes

## 2020-03-26

* [x] Bye bye Firebase and the reliance on third-party cookies: authenticate to TrustInSoft CI directly via GitHub
* [x] Easy configuration: on the Add project page, click on the Copy button to copy-paste a sample [tis.config](reference/configuration-file.md) file and add it to your project
* [x] Easy sharing: copy-paste test URLs and share with others by clicking on the Hyperlink button
* [x] After launch, TrustInSoft CI Analyzer now automatically points to the code statement where the Undefined behavior was found
* [x] If your project or GitHub organization changes name, TrustInSoft CI automatically updates the database so the change is transparent to you
* [x] Other small improvements and bug fixes

## 2020-02-26

* [x] Live chat is ON: feel free to ask all of your questions! Info on schedule [here](get-help.md)
* [x] Easy configuration: there is a new Config tab that lets you view each of your test configuration without having to switch to github.com
* [x] Other small improvements and bug fixes

