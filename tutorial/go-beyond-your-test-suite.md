---
description: >-
  On the previous page, we proved the absence of undefined behaviors on the
  test.
---

# Go beyond your test suite

## Add more tests

As you have successfully verified your first test, you may stop the configuration effort here if you are happy with the coverage of this test. After each new commit pushed to GitHub, TrustInSoft CI will keep verifying the test to check for non-regression.

Alternatively, you may add more tests to the tis.config file to catch more undefined behaviors and put more trust in your code.

### Look at dead statements

**1.** Still on the analysis history page for demo-caesar, click on the new analysis that shows no undefined behavior:

![](../.gitbook/assets/image%20%28121%29.png)

**2.** Then, click on **Explore** on this second test analysis to explore the new result.

**3.** Once TrustInSoft CI Analyzer has loaded the new analysis, take a look at the **Overview panel**, which provides a few statistics about our test. In particular, the statement coverage is 94%, meaning that some statements are not reached by the test so not tested:

![](../.gitbook/assets/image%20%286%29.png)

**4.** Click on the **Statements** **tab** \(bottom\) and set the **Reachability filter** to **unreachable** to see all the dead statements. 6 statements were not reached:

![](../.gitbook/assets/image%20%2872%29.png)

**5.** Click on the last statement in file **caesar.c line 60** in the list. The **Interactive code panel** highlights this statement in blue:

![](../.gitbook/assets/image%20%28186%29.png)

From the code, it looks like a test that takes as input, a string with a character "greater" than "Z", should be added to reach that statement.

**6.** You can do the same exercise with the other dead statements highlighted in red.

### Test the boundaries

Recall that demo-caesar is intended to be used with any input strings or shift values. Can you guess what you would get with the shift\_value `INT_MIN`?

Another undefined behavior as shown here:

![](../.gitbook/assets/image%20%28111%29.png)

`abs_x` is a signed integer so can only hold values between `-2147483648` and `2147483647`. Since `x` contains the shift value `INT_MIN = -2147483648`, calculating the opposite of `x` causes a signed overflow.

Testing with input values approaching or crossing the boundaries is an effective way of finding new undefined behaviors.

### Use fuzzing

Finally, you may also use fuzzing to automatically generate lots of relevant inputs.

## Generalize your test suite

### Motivation

In formally verifying your tests, TrustInSoft CI goes far beyond "classic" testing, since it can detect the most subtle undefined behaviors, even when applied to well-tested code that has never revealed any problem.

To increase statement and value coverage, you can always specify more tests in the tis.config file.

But what if you would like to both achieve **full statement and value coverage** and stick to a decent amount of tests and analyses?

The solution is called **test generalization**. It consists in "relaxing" the inputs of your **existing tests** by specifying, as test inputs, ranges or sets of values instead of single values.

### How it works

{% hint style="info" %}
Test generalization is not available in TrustInSoft CI yet so this section shows a preview.
{% endhint %}

Let's come back to our project [demo-caesar](https://github.com/TrustInSoft-CI/demo-caesar).

#### Generalize with all shift values

We know from [Test the boundaries](go-beyond-your-test-suite.md#test-the-boundaries) that the shift value `INT_MIN` reveals an undefined behavior.

To be fully confident that the user of this library will be able to choose any integer as shift value, let's say that we'd like to test and verify with all possible integer values.

Instead of creating 2^32 tests, we can re-use the current test and specify as shift value, with the special function `tis_interval`, an integer range between `INT_MIN` and `INT_MAX` instead of a single integer value:

```text
int main(void)
{
    ...
    printf("\nTest 3: All shift values\n");
    int shift = tis_interval(INT_MIN, INT_MAX);
    gen_test(orig_str, str_len, shift);
    ...
}
```

After running the analysis again, the **Values widget** shows, with the notation `--`, that the shift value has been initialised to the interval `[INT_MIN, INT_MAX]`:

![](../.gitbook/assets/image%20%28137%29.png)

The **Properties widget** shows that **3 undefined behaviors** have been detected:

![](../.gitbook/assets/image%20%28150%29.png)

The first row links to the undefined behavior described in [Test the boundaries](go-beyond-your-test-suite.md#test-the-boundaries). It implies that the user should be prevented from choosing `INT_MIN` as shift value.

But what about the two other undefined behaviors? They both reveal a signed overflow, but on another part of the code as shown here:

![](../.gitbook/assets/image%20%28170%29.png)

The absolute value `abs_shift` of the input shift can take any value between `0` and `2147483647` and the input string is made of `char` values between `69` and `80` . Adding the two together triggers a signed overflow.

Let's assume that you've implemented a fix. The statement coverage increases to 96%. As you've tested with all input shifts, you are on the right path, but not fully there yet!

#### Generalize with all input strings

You know from [Look at dead statements](./) that some statements are not reached because the input string is missing some special characters.

Same as above. Instead of verifying on a limited number of additional input strings, you can specify, with the special function `tis_make_unknown`, an abstract input string representing all possible char arrays of a given size:

{% code title="" %}
```text
int main(void)
{
    ...
    printf("\nTest 4: All shift values and all string inputs\n");
    tis_make_unknown(orig_str, str_len-1);
    gen_test(orig_str, str_len, shift);
    ...
}
```
{% endcode %}

Let's run the analysis again and observe the results. There's no undefined behaviors. 

Assuming the library is meant to be used with any shift value between `-INT_MAX` and `INT_MAX` and any input string of length `39`, you have formally proved it is free of undefined behaviors!

As bonus, we get a statement coverage of 100%:

![](../.gitbook/assets/image%20%28104%29.png)

{% hint style="success" %}
We introduced the special functions`tis_internal and tis_make_unknown`, which are part an API. You may also have noticed that the above **Overview panel** looks quite different than on TrustInSoft CI Analyzer.

These are part of **test generalization**, a **unique feature**, that is not yet available in TrustInSoft CI, but that we are already offering to our on-premise customers. 

[Contact us for more info](../get-help.md)
{% endhint %}

