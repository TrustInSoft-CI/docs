# Prerequisites

## Introduction to Cxx\_matrix

The tutorial is based on the C++ project [Cxx\_matrix](https://github.com/TrustInSoft-CI/Cxx_matrix). 

Its source code on GitHub contains 2 source files:

* `matrix.h` that is a matrix operations library coded in C++
* `matrix.cpp` that is a simple test over this library

Let's familiarize ourselves with the `main` function in `matrix.cpp`:

```cpp
int
main(void) {
    Matrix<2U, 2U> matrix_a {
        2., 1.,
        4., 2. };

    auto id = identity<2>();
    bool has_inverse = is_invertible(id);
    std::cout << "identity is inversible: " << (has_inverse ? "yes\n" : "no\n");

    Matrix<2U, 2U> matrix_b = matrix_a + (5 ^ id);
    Matrix<2, 1> res = solve(matrix_b,  { 6., 10. });
    std::cout << "RESULT IS:\n" << res;

    return 0;
    (void) has_inverse;
}
```

The test implemented in `main` performs the following step:

* It initializes a 2 x 2 matrix
* It verifies if the matrix is invertible
* It performs some basic operations on this matrix
* It solves a matrix equation

## Launching the analyzer

We already configured the project [Cxx\_matrix](https://github.com/TrustInSoft-CI/Cxx_matrix) for TrustInSoft CI by adding a [`tis.config`](https://github.com/TrustInSoft-CI/Cxx_matrix/blob/master/tis.config) file to the repository. In our example, the `tis.config` file specifies the source file `matrix.cpp` to analyze, the analysis entry point function `main` and the preprocessor option `-I.`.

We also launched the analysis in TrustInSoft CI:

**1.** Load the following URL in your browser to vizualize a summary of the analysis results:   
[https://ci.trust-in-soft.com/projects/TrustInSoft-CI/Cxx\_matrix/latest](https://ci.trust-in-soft.com/projects/TrustInSoft-CI/Cxx_matrix/latest)

There is only one test, as one entry point, and no undefined behavior on the tested path:

![](../.gitbook/assets/image%20%28122%29.png)

**2**. Sign in to TrustInSoft CI if you're not already signed in. For more information, check out our [introduction tutorial](../tutorial/).

**3.** Launch TrustInSoft CI Analyzer by clicking the **Explore** button.

Next let's dive into C++ identifiers, constructions and calling conventions in TrustInSoft CI Analyzer.

