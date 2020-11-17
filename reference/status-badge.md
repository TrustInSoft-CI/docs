# Add a status badge

## Quick guide

Add a status badge ![](../.gitbook/assets/screenshot-2020-07-08-at-12.05.23.png) to your project's README by copying and pasting the below markdown template and inserting into it your project name and corresponding GitHub account: 

`[![TrustInSoft CI](https://ci.trust-in-soft.com/projects/<ACCOUNT_NAME>/<PROJECT_NAME>.svg?branch=master)](https://ci.trust-in-soft.com/projects/<ACCOUNT_NAME>/<PROJECT_NAME>)`

## More about the badge

A badge status is computed based on the statuses of the tests that belong to the **latest** **valid** verification. A verification is **valid** if its result is complete, meaning that the verification has not been canceled nor it is queued/running.

There are 3 possible statuses:

| Status | Definition |
| :--- | :--- |
| Passing | All of the tests in the latest valid verification have the status "No undefined behavior". |
| Failing | At least one test in the latest valid verification does not have the status "No undefined behavior" so it should be fixed. |
| Unknown | There is not valid verification for this project/branch. |



