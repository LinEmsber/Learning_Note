# GCC floating options

## Options

### -ffast-math
  * Sets the options -fno-math-errno, -funsafe-math-optimizations, -ffinite-math-only, -fno-rounding-math, -fno-signaling-nans, -fcx-limited-range and -fexcess-precision=fast.

  * This option causes the preprocessor macro __FAST_MATH__ to be defined.

  * This option is not turned on by any -O option besides -Ofast since it can result in incorrect output for programs that depend on an exact implementation of IEEE or ISO rules/specifications for math functions. It may, however, yield faster code for programs that do not require the guarantees of these specifications.

### -fsingle-precision-constant
  * Treat floating-point constants as single precision instead of implicitly converting them to double-precision constants.

## Reference
  * [GCC Optimize Options](https://gcc.gnu.org/onlinedocs/gcc/Optimize-Options.html)
  * [What does gcc's ffast-math actually do?](https://stackoverflow.com/questions/7420665/what-does-gccs-ffast-math-actually-do)
  * [Semantics of Floating Point Math in GCC](https://gcc.gnu.org/wiki/FloatingPointMath)
