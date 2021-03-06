# ComplexNumber

A simple and comprehensive complex number class in PHP.  MIT Licensed

## Overview

- Version: `v1.0.0` Stable
- Owner: [DonaldKellett](https://github.com/DonaldKellett)
- License: MIT License

### Why use *this* implementation?

Although this project most definitely isn't the first PHP complex number handling software out there, here are just a few reasons why you should use this implementation over most other common implementations:

1. Immutability - The `ComplexNumber` class in this project has been carefully designed such that no amount of operations can alter the instance of the complex number itself and it is impossible to reassign a different value to an instance of `ComplexNumber` without raising an error/exception of some kind.  This means that it is impossible for you to accidentally change the "value" of a complex number itself and cause weird, unexpected behavior - the very nature of the class/instance prevents you from doing so.  This is also the behavior that makes most sense as you can't change the properties of a *primitive value* (e.g. an integer or float in PHP), can you?
2. Strict input validation - Each instance and class method in this implementation always type checks its arguments to ensure that you are not passing `"Hello World"` into a strictly mathematical `ComplexNumber::exp()` function or trying to create a complex number with value `true + array(3, 5, "Goodbye World")i` :wink:
3. Mathematician-friendly - Many of the instance and class methods of this implementation use naming conventions and notation that are familiar to hardcore mathematicians to avoid any possible confusion, such as `ComplexNumber::Im()` and `ComplexNumber::arg()` (which return the imaginary component and argument of a complex number respectively).
4. Ease of readability - the large number of aliases for certain primitive operations (such as `ComplexNumber::times()` and `ComplexNumber::multipliedBy()` for `ComplexNumber::multiply()`) means that you can use different versions of the same operation in your code depending on the context to make it read like plain English and not some cryptic Martian language.
5. Lenient input types - All methods in the `ComplexNumber` class that accept one or more arguments treat integers, floats and complex numbers as, well, *numbers* - there is no distinction between the three.
6. Consistent return types - On the other hand, no matter what type of number you pass into certain methods of the `ComplexNumber` class, you are always guaranteed to receive a new instance of `ComplexNumber` as output so you can safely chain any number of instance/class methods in your calculations, so long as you don't divide by zero or make another type of math error.  If you *do* divide by zero, well, I'm afraid I can't help you :wink:  The only exceptions to this rule are the ones that extract crucial information from the complex number itself - these always return a real number (integer/float).
7. Powerful mathematical functions beyond primitive operations - While *most* complex number implementations out there go no further than, perhaps, the `sqrt()` function, this implementation goes well beyond that and supports serious mathematical operations such as the generalized exponential and logarithmic functions.

## File Synopsis

Filename | Description
--- | ---
`LICENSE` | The MIT License
`README.md` | Documentation
`src/` | Source code for the `ComplexNumber` class
`src/class.complexnumber.php` | File containing the `ComplexNumber` class
`test/` | Directory containing the test cases for this project using [PHPTester](https://github.com/DonaldKellett/PHPTester) as the TDD framework
`test/test_cases.php` | The entire test suite for this project
`test/PHPTester-3.1.0/*` | The PHPTester testing framework (version 3.1.0)

## PHP Version

The entire project has been tested and confirmed to work properly in all versions of PHP 7.  Furthermore, the `ComplexNumber` class should also work in PHP 5.6.x+ but this has yet to be officially confirmed.  The PHPTester testing framework (version 3.1.0) requires PHP 7 or later but that is irrelevant to this project (except for viewing the passing assertions).

## Contributing

Contributions are now open and welcome :smile:  This can take one of a few possible forms:

### Filing an Issue Report

Found a bug in the source code?  Found a typo?  Or would you like a feature implemented but don't have the time to make a pull request?  If that is the case, then filing an issue report on this Repo is the way to go.

### Making a Pull Request

If you've found and fixed a bug, fixed up a few typos, or straight up implemented a whole new feature, you may make a pull request.  However, there are a few conditions for a pull request to be approved (as long as any *code* changes are involved - fixing typos do not need to pass these tests):

- Any currently existing assertions should not be broken
- The current test cases should not be modified, unless for a good reason (such as a behavioral change in an already-implemented method)
- Relevant, new assertions should be added - the more the better, and should also include at least a few edge cases
- The new assertions you added should pass (obviously)
- You must ensure that any newly implemented methods do not mutate any of its arguments or the instance itself - you may want to write a few extra tests to confirm that
- In case of any disputes, both the behavior and the test cases included for everything should agree with the results computed by [WolframAlpha](http://wolframalpha.com).  Alternatively, if there is a specified behavior in a core PHP function that parallels the method you are implementing (such as `pow(0, 0)` returning `1` in the PHP core), you may want your method(s) to agree with that instead.

On top of all that, there is a particular type of pull request that will **always be rejected** - complex number to string conversion (or vice versa).  This is because how a `ComplexNumber` object should be represented in real life is debatable, e.g. should `new ComplexNumber(0, 1)` be displayed as `i`, `1i`, `0 + i` or `0 + 1i`?  If you really want this feature, you should create a separate project that implements this.

## Class Synopsis (ComplexNumber)

### Class Constants

```php
ComplexNumber::RECTANGULAR_FORM === 0;
ComplexNumber::MODULUS_ARGUMENT_FORM === 1;
```

Each class constant is assigned a numerical value but it is **not recommended** to use them for purposes other than those mentioned in the documentation below to avoid any possible confusion.

### Initializing a Complex Number (Class Constructor)

The class constructor has two main forms.

#### Rectangular Form (z = x + iy)

```php
ComplexNumber::__construct(mixed $x[, mixed $y = 0]);
```

Initializes a complex number of the form `z = x + iy` (rectangular/Cartesian coordinates).  Both arguments provided must be either an integer or a float - any attempt to pass in other data types with result in an `InvalidArgumentException`.  If the second argument `$y` is not provided, it is assumed to be `0`.  E.g.

```php
new ComplexNumber(3, 4); // 3 + 4i
```

#### Modulus-argument form (z = re^(i * theta))

```php
ComplexNumber::__construct(mixed $r, mixed $theta, int $form === ComplexNumber::MODULUS_ARGUMENT_FORM);
```

If a third argument is provided to the constructor and set equal to `ComplexNumber::MODULUS_ARGUMENT_FORM`, then the first 2 arguments will be treated as `$r` and `$theta` respectively, where `$r` is the modulus of `z` and `$theta` its argument (`arg(z)`).  Again, both arguments must be numbers, and additionally, any `$r < 0` and any `$theta` outside the range `(-PI, PI]` will throw an `InvalidArgumentException`.  E.g.

```php
new ComplexNumber(5, M_PI / 6, ComplexNumber::MODULUS_ARGUMENT_FORM); // 5e^(i * PI / 6) = 2.5 * sqrt(3) + 2.5i
```

#### Any other third argument

If a third argument is provided which is neither of `ComplexNumber::RECTANGULAR_FORM` and `ComplexNumber::MODULUS_ARGUMENT_FORM` then an `InvalidArgumentException` will be thrown.

### Fundamental Properties - Instance and Class Methods

#### getImaginary

```php
mixed ComplexNumber::getImaginary()
```

An instance method that receives no arguments and returns the imaginary component of the complex number either as an integer or as a float.  For example:

```php
$z = new ComplexNumber(2, 5); // 2 + 5i
$z->getImaginary(); // => 5
```

#### Im(z)

```php
mixed ComplexNumber::Im(mixed $z)
```

A **static class method** that is essentially an alias of `ComplexNumber::getImaginary()` but receives the real or complex number `$z` as its one and only argument.  Returns the imaginary component of `$z` as an integer or float.  If `$z` is anything but a number (real or complex), an `InvalidArgumentException` is thrown.  E.g.

```php
$z = new ComplexNumber(-12, -5); // -12 - 5i
ComplexNumber::Im($z); // => -5
```

#### getReal

```php
mixed ComplexNumber::getReal()
```

An instance method that receives no arguments and returns the real component of the complex number either as an integer or as a float.  E.g.

```php
$z = new ComplexNumber(-7, 24); // -7 + 24i
$z->getReal(); // => -7
```

#### Re(z)

```php
mixed ComplexNumber::Re(mixed $z);
```

A **static class method** that accepts 1 argument `$z`, a real or complex number, and returns its real component as an integer or float.  If `$z` is not a number, an `InvalidArgumentException` is thrown.  For example:

```php
$z = new ComplexNumber(1, 3); // 1 + 3i
ComplexNumber::Re($z); // => 1
```

#### getModulus

```php
float ComplexNumber::getModulus()
```

An instance method that receives no arguments and returns the modulus of the complex number as a float.  E.g.

```php
$z = new ComplexNumber(1, sqrt(3));
$z->getModulus(); // => 2.0 (approx.)
```

#### abs(z) (i.e. |z|)

```php
float ComplexNumber::abs(mixed $z)
```

A **static class method** that receives exactly 1 argument, `$z` (a real or complex number), and returns its absolute value (i.e. modulus) as a float.  E.g.

```php
$z = new ComplexNumber(3, 4); // 3 + 4i
ComplexNumber::abs($z); // => 5.0 (approx.)
```

#### getArgument

```php
float ComplexNumber::getArgument()
```

An instance method that receives no arguments and returns the argument of the complex number `arg(z)` as a float in the range `(-PI, PI]`.  E.g.

```php
$z = new ComplexNumber(sqrt(2), M_PI / 4, ComplexNumber::MODULUS_ARGUMENT_FORM); // sqrt(2) * e^(i * PI / 4)
$z->getArgument(); // => PI / 4 (approx.)
```

*NOTE: When the complex number is zero, its argument is considered to be* `0` *in agreement with Wolfram Alpha.*

#### arg(z)

```php
float ComplexNumber::arg(mixed $z)
```

A **static class method** that accepts one argument `$z`, a real or complex number, and returns its argument `arg(z)` which is a float in the range `(-PI, PI]`.  For example:

```php
$z = new ComplexNumber(3, M_PI / 2, ComplexNumber::MODULUS_ARGUMENT_FORM); // 3e^(i * PI / 2)
ComplexNumber::arg($z); // => PI / 2 (approx.)
```

*NOTE: When the complex number is zero, its argument is considered to be* `0` *in agreement with Wolfram Alpha.*

#### getComplexConjugate

```php
ComplexNumber ComplexNumber::getComplexConjugate()
```

An instance method that receives no arguments and returns its complex conjugate `z*` as a new instance.  E.g.

```php
$z = new ComplexNumber(1, 1); // 1 + i
$z->getComplexConjugate(); // 1 - i
```

### Primitive Operations

#### add

```php
ComplexNumber ComplexNumber::add(mixed $z)
```

An instance method which receives exactly 1 argument, `$z` (an integer / float / complex number) and adds it to the current complex number.  Returns a new instance; the original instance is unchanged.  E.g.

```php
$z = new ComplexNumber(1, 2); // 1 + 2i
$z->add(3); // => 4 + 2i
$z->add(-3 / 2); // => -0.5 + 2i
$z->add(new ComplexNumber(3, -7)); // => 4 - 5i
```

#### plus

```php
ComplexNumber ComplexNumber::plus(mixed $z)
```

An alias of `ComplexNumber::add()`

#### subtract

```php
ComplexNumber ComplexNumber::subtract(mixed $z)
```

An instance method that accepts exactly 1 argument, `$z` (an integer / float / complex number), and returns a new complex number equivalent to `$z` subtracted from the current instance.  For example:

```php
$z = new ComplexNumber(5, 6); // 5 + 6i
$z->subtract(new ComplexNumber(-10, 10)); // => (5 + 6i) - (-10 + 10i) = 15 - 4i
```

#### minus

```php
ComplexNumber ComplexNumber::minus(mixed $z)
```

An alias of `ComplexNumber::subtract()`

#### multiply

```php
ComplexNumber ComplexNumber::multiply(mixed $z)
```

An instance method that accepts exactly 1 argument `$z` (an integer / float / complex number) and returns a new complex number which is the product of the two complex numbers.  E.g.

```php
$z = new ComplexNumber(1, 2); // 1 + 2i
$w = new ComplexNumber(3, 4); // 3 + 4i
$z->multiply($w); // => (1 + 2i) * (3 + 4i) = -5 + 10i
```

#### times

```php
ComplexNumber ComplexNumber::times(mixed $z)
```

An alias of `ComplexNumber::multiply()`

#### multipliedBy

```php
ComplexNumber ComplexNumber::multipliedBy(mixed $z)
```

An alias of `ComplexNumber::multiply()`

#### divide

```php
ComplexNumber ComplexNumber::divide(mixed $z)
```
An instance method that receives exactly 1 argument `$z` (a nonzero integer / float / complex number) and returns a new complex number equivalent to the current instance divided by `$z`.  If the provided argument is zero (`0` / `0.0` / `0 + 0i`), throws a `DivisionByZeroError`.  For other invalid input (e.g. string, boolean, array), throws an `InvalidArgumentException`.  E.g.

```php
$z = new ComplexNumber(10, 5); // 10 + 5i
$z->divide(new ComplexNumber(1, 2)); // => 4 - 3i
```

#### over

```php
ComplexNumber ComplexNumber::over(mixed $z)
```

Alias of `ComplexNumber::divide()`

#### dividedBy

```php
ComplexNumber ComplexNumber::dividedBy(mixed $z)
```

Alias of `ComplexNumber::divide()`

### Common Mathematical Functions

#### sqrt

```php
ComplexNumber ComplexNumber::sqrt(mixed $z)
```

A **static class method** that receives exactly 1 argument `$z` (an integer / float / complex number) and returns its square root as a complex number.  E.g.

```php
ComplexNumber::sqrt(25); // => 5 + 0i (an instance of ComplexNumber)
ComplexNumber::sqrt(-1); // => 0 + i
$z = new ComplexNumber(-7, 24); // -7 + 24i
ComplexNumber::sqrt($z); // => 3 + 4i
```

### Exponential/Logarithmic Functions

#### exp

```php
ComplexNumber ComplexNumber::exp(mixed $z)
```

A **static class method** that accepts exactly 1 argument `$z` (an integer, a float or a complex number) and computes the result of <em>e<sup>z</sup></em>.  Returns the result as a new instance of `ComplexNumber`.  E.g.

```php
$z = new ComplexNumber(2, 3); // 2 + 3i
ComplexNumber::exp($z); // => e^(2 + 3i) = e^2 * e^(3i) = e^2 * (cos(3) + isin(3))
```

#### log

```php
ComplexNumber ComplexNumber::log(mixed $z[, mixed $base = M_E])
```

A **static class method** that accepts one to two arguments, the first being `$z` (an integer, float or complex number), the exponent of the operation, and the second being `$base` (an integer, float or complex number), the base of the operation.  Returns a new instance of `ComplexNumber` which is the result of <em>log<sub>(base)</sub>z</em>.  If the base is not provided, it is assumed to be `e = 2.718281828459045 ... `.  E.g.

```php
$z = new ComplexNumber(24, 7); // 24 + 7i
$b = new ComplexNumber(-3, -4); // -3 - 4i
ComplexNumber::log($z, $b); // => Logarithm of (24 + 7i) to base (-3 - 4i)
```

#### pow

```php
ComplexNumber ComplexNumber::pow(mixed $z, mixed $w)
```

A **static class method** that given two real/complex numbers `$z` and `$w`, evaluates <em>z<sup>w</sup></em> and returns the result as a new instance of `ComplexNumber` even if both arguments passed in are real numbers.  When `z = 0 (= 0 + 0i)`, <em>z<sup>w</sup></em> is considered to be `0 + 0i` as well as long as `Re(w) > 0`.  When `Re(w) = 0`, <em>z<sup>w</sup></em> is considered to be `1 + 0i` regardless of the imaginary part of `w` (where `z = 0 + 0i`).  Otherwise, when `z = 0` and `Re(w) < 0`, an `ArithmeticError` is thrown.  E.g.

```php
ComplexNumber::pow(new ComplexNumber(3, 4), new ComplexNumber(-7, 24)); // => (3 + 4i) ^ (-7 + 24i)
```
