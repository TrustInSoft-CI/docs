# Identifiers, constructors and calling conventions

After opening TrustInSoft CI Analyzer, take a look at the **Overview panel** on the left hand side, that confirms there are no undefined behaviors.

Since there are no undefined behaviors, the **Interactive code panel** in the middle displays by default the `main` function:

![](../.gitbook/assets/image%20%2816%29.png)

The **Interactive code panel** displays pseudo-C code that preserves the exact semantics of the original C++ code that is analyzed. The differences with the C language and how the original C++ code was translated to this pseudo-C code are explained below.

The first thing to notice is that some names contain characters that are forbidden in C, like `Matrix<2, 2>` or `std::basic_ostream`, and may be prefixed by `...`. Names of entities in C++ code are actually mangled, the **Interactive Code panel** displays an unmangled version of them to be clearer.

When a name is too long, a shortened version of it is displayed in the **Interactive code panel** with `...` as prefix. Clicking on this prefix will display the full qualified name.

The first statement that is not a declaration is a call to the function `__tis_globinit()`. This function represents the dynamic initialization phase of a C++ program \(as stated in [\[basic.start.init\]](https://timsong-cpp.github.io/cppwp/n3337/basic.start.init)\).  It contains only calls to functions with names similar to `X::Y::__tis_init_Z`, that are used to initialize the non-local variables `X::Y::Z`.

Looking at the definition of the `X::Y::__tis_init_Z` function will lead the **Source Code panel** to the body of the generated function initializing the variable `X::Y::Z`

The first statement of the `main` function in the source code is:

```text
    Matrix<2U, 2U> matrix_a {
        2., 1.,
        4., 2. };
```

and corresponds in the normalized code to the line:

```text
  Matrix<2, 2>::Ctor<double, double, double, double>(& matrix_a,2.,1.,4.,2.);
```

`Ctor` is the generic name that TrustInSoft CI assigns to C++ constructors and you can see that:

* the constructor templated arguments are made explicit.
* all initializers of the source code are passed as arguments.
* there is an additional argument `& matrix_a`.

All method calls are translated to regular C function calls, and as such they receive an additional argument which stands for the `this` pointer. In case of constructors, this is the address of the object being initialized.

When looking at the constructor definition, you can see that it is calling the inherited constructor `Matrix_base<2, 2>::Ctor<double, double, double, double>` with the same arguments, except that the `this` pointer is shifted to its `__parent__Matrix_base<2, 2, Matrix>`. 

The corresponding part of the original code is:

```text
        const Matrix_base<I, J, Parent> &m1,
        const Matrix_base<J, K, Parent> &m2)
{
    auto cross_product =
        [&m1, &m2] (unsigned i, unsigned j) -> double
        {
```

`Matrix<N, M>` inherits from `Matrix_base<N, M, Matrix>`, and its constructor only transfers its arguments to the constructor of the parent class. On TrustInSoft CI, a class `A` inheriting from a class `B` is represented by a `struct A` containing a field `struct B __parent__B`. The initialization of the base `B` of `A` is translated into a call the function `B::Ctor(&A.__parent__B)`. This structure layout can be observed in the example by looking at the definition of the type `struct Matrix<2, 2>`.

The next statement of the `main` function in the original code is:

```text
    auto id = identity<2>();
```

and corresponds in the normalized code to the line:

```text
  identity<2>(& id);
```

The first thing to note is that the `id` variable has an `auto` type in the original source but is declared in the normalized code as:

```text
  struct Matrix<2, 2> id;
```

TrustInSoft CI makes `auto` types explicit, in the same way it instantiates template parameters.

Another difference is that in the normalized code the `identity<2>` function takes an additional argument despite being a usual function and not a method. This is a consequence of the fact that, in C++, a non-POD \(in the sense of [\[class\]p10](https://timsong-cpp.github.io/cppwp/n3337/class#10)\) value returned by a function may not live inside the function but inside its caller. To model this, a function returning a non-POD type receives an additional parameter which contains the address of the initialized object.

The next statement of the `main` function in the original code is:

```text
    bool has_inverse = is_invertible(id);
```

which, when clicked on, corresponds in the normalized code to:

```text
  {
    struct Matrix<2, 2> __tis_arg;
    _Bool tmp;
    {
      {
        Matrix<2, 2>::Ctor(& __tis_arg,(struct Matrix<2, 2> const *)(& id));
        tmp = is_invertible<2>(& __tis_arg);
      }
      has_inverse = tmp;
    }
  }
```

In this case, one C++ statement is translated into a block containing multiple declarations and statements. The function `is_invertible<2>` takes its argument by copy, as seen in its declaration:

```text
template<unsigned N>
bool
is_invertible(Matrix <N, N> m)
```

and so its parameter has to be initialized with a new object. This is the purpose of the `__tis_arg_*` family variables. In the current case, `__tis_arg` is initialized by calling the copy constructor of `Matrix<2, 2>` with the address of `id` as source. Then, the address of the newly built `__tis_arg` variable is given to the function `is_invertible<2>` and the block around it delimits the lifetime of `__tis_arg`. This is the semantics of passing arguments by copy, see [\[expr.call\]p4](https://timsong-cpp.github.io/cppwp/n3337/expr.call#4) and in particular: “The initialization and destruction of each parameter occurs within the context of the calling function.”

This transformation does not happen when calling the copy constructor of `Matrix<2, 2>` because its argument is a reference. References are converted to pointers, so taking a reference to an object is taking its address, and accessing a reference simply dereferences the pointer.

The next interesting statement is the one at line 37 of the original source:

```text
    Matrix<2U, 2U> matrix_b = matrix_a + (5 ^ id);
```

which is translated in the normalized code as:

```text
  {
    struct Matrix<2, 2> __tis_tmp_61;
    {
      {
        operator^(& __tis_tmp_61,(double)5,
                  (struct Matrix<2, 2> const *)(& id));
      }
      ;
      ;
    }
    operator+(& matrix_b,
              (struct Matrix_base<2, 2, Matrix> const *)(& matrix_a.__parent__Matrix_base<2, 2, Matrix>),
              (struct Matrix_base<2, 2, Matrix> const *)(& __tis_tmp_61.__parent__Matrix_base<2, 2, Matrix>));
  }
```

Again, this statement is decomposed into a block containing multiple statements but declaring this time a variable called `__tis_tmp_62`. The `__tis_tmp_*` family of variables correspond to temporary object \(as defined in [\[class.temporary\]](https://timsong-cpp.github.io/cppwp/n3337/class.temporary)\) that can be introduced by complex expressions. This temporary object is declared inside the block as its lifetime is the one of the full expression, and has to be destroyed at its end if needed.

