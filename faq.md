# FAQ

## What is TrustInSoft CI?

TrustInSoft CI is an online source code analyzer that continuously detects undefined behaviors in C and C++ programs \(crash, arbitrary code execution, ...\).

 It is available in beta for open-source and public projects hosted on GitHub and it is all free!

## Who can use TrustInSoft CI?

TrustInSoft CI is targeted at GitHub C or C++ developers and project maintainers, who develop or maintain security-sensitive code.

## Why use TrustInSoft CI?

**It can find more undefined behaviors** 

TrustInSoft CI works at the source code level and relies on the latest [formal methods](https://en.wikipedia.org/wiki/Formal_methods) to understand the code statement by statement. It can therefore detect the most subtle violations of the C and C++ standards, even when applied to regression tests that have never revealed any problem.

**It makes it easy to find their root cause**

TrustInSoft CI ships with a powerful debugging interface, that lets developers explore all the values of all the variables and troubleshoot their own code or understand third-party code behavior.

**It natively integrates with GitHub and is CI ready**

Hence, there is only an initial setup for the entire project lifecycle, and all the project's contributors and maintainers get the value.

## What is an undefined behavior?

Undefined behaviors are defined by the C and C++ standards. They usually correspond to illegal operations and may lead to crashes and security vulnerabilities. Their effects are also highly dependent on the interactions with the compilers and their optimizations.

TrustInSoft CI detects all major families of undefined behaviors including but not restricted to buffer overflow, dangling pointer, invalid pointer operation, division by zero, uninitialized memory read and arithmetic overflow. [Click here](reference/cwe-coverage.md) to learn more about CWE coverage of TrustInSoft CI.

## How to configure a project?

Several steps are involved. Amongst them, you should write a short configuration file, that lists the analysis entry points \(usually the project tests\), the source files to analyze and the compilation options for parsing them. You should also give TrustInSoft CI access to your GitHub repository.

To learn more the exact steps, you can either [follow our introduction tutorial](tutorial/) or directly [sign in](https://ci.trust-in-soft.com/) and read the welcome page.

## When do analyses get triggered? <a id="trigger"></a>

TrustInSoft CI triggers an analysis each time a new commit is pushed to GitHub on a branch with the configuration file. 

Pull request triggers are not supported yet. [Contact us](get-help.md) to get the latest information.

## Is there a limit on the number of analyses?

There is a limit of 2 concurrent analyses per GitHub account \(over all projects\).

## What are the technical requirements? <a id="technical-requirements"></a>

TrustInSoft CI can analyze projects or branches that are: 

* Public on GitHub and written in C or C++
* Equipped with at least one test case or entry point
* Self-contained: all the relevant source files \( `.c` ,`.cpp` , `.h` , ...\) including dependencies must be available in the GitHub repository

{% hint style="info" %}
TrustInSoft CI will stop the analysis and emits an error, as soon as it encounters a function, whose body is located outside of the GitHub repository \(the project is not self-contained\), or a piece of code that is not written in C or C++.
{% endhint %}

## Do I need specific GitHub permissions? <a id="github-permissions"></a>

GitHub owner \(admin\) rights over the project are required for setting up the continuous analysis of such project. The same permissions are required for canceling or restarting an analysis, or removing a project. [Read about organization projects](faq.md#github-organization)

## Are organization projects supported? <a id="github-organization"></a>

Yes, TrustInSoft CI can analyze organization projects. [GitHub organizations](https://help.github.com/en/github/setting-up-and-managing-organizations-and-teams/about-organizations) are shared GitHub accounts where businesses and open-source teams can collaborate on the same projects.

The steps for setting up the continuous analysis of such project are the same as for an individual project, except you must be an owner of the GitHub organization and TrustInSoft CI must have access to this organization. [Read the article](reference/github-organizations.md)

## How to add an organization? <a id="how-to-add-an-organization"></a>

Adding an organization to TrustInSoft CI is required for analyzing projects that belong to a GitHub organization account. For this, you must be an owner of the organization and grant TrustInSoft CI access to this organization. [Read the article](reference/github-organizations.md)

## Are private projects supported? <a id="private-projects"></a>

Not yet. [Contact us](get-help.md) to get the latest information.

