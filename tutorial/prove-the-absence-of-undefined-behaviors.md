---
description: >-
  On the previous page, we found the root cause of the detected undefined
  behavior.
---

# Prove the absence of undefined behaviors

## Fix the undefined behavior

{% hint style="info" %}
Proving the absence of undefined behaviors is straightforward with TrustInSoft CI =&gt; fix all the undefined behaviors until the test shows "No undefined behaviors".

After this point, you may relax and wait for the next code change and analysis result or decide to [go beyond your test suite](go-beyond-your-test-suite.md).
{% endhint %}

On the previous page, we realised that the code should not allocate a buffer of constant size but one of the same size as the input string. Fixing this should remove the undefined behavior.

To save you some time, we've prepared the code change in a dedicated branch called **fix-memory-alloc**. 

You'll merge this branch to your master branch via a pull request. Here are the steps with a screen recording at the end:

**1.** On GitHub, click on **New pull request** on your fork project.

**2.** Change the **compare branch** to **fix-memory-alloc**.

**3.** Change the **base repository** to your **fork project**.

**4.** Review the changes in the diff.  
  
The `caesar_encrypt` and `caesar_decrypt` functions take the length of the input string as an additional argument, and the code allocates a buffer `buf` of the same size `str_len` as the input string.

**5.** Click on **Create pull request**, and again to confirm.

**6.** Click on **Merge pull request**, then **Confirm merge**.

![](../.gitbook/assets/2019-10-16_16-37-44-1-.gif)

You have just merged the code change to your master branch!

## Observe the updated analysis result in TrustInSoft CI

**1.** In TrustInSoft CI, click on **demo-caesar**:

![](../.gitbook/assets/image%20%28167%29.png)

You will be redirected to the analyses history of this project.

**2.** Observe that a new analysis was automatically triggered on the master branch:

![](../.gitbook/assets/image%20%2887%29.png)

{% hint style="success" %}
Since no undefined behavior was found, you have proved the absence of undefined behaviors on the tested path!
{% endhint %}

