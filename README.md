# DynaCasting

Differences between implicit and explicit casting in strongly typed languages such as C and dynamic languages such as Python.
---

## Explixit casting 

### C

```C
#include <stdio.h>

int main() {
    int a;
    float b;

    a = 1;
    b = (float) a;

    printf("%d\n", a); // Output: 1
    printf("%f\n", b); // Output: 1.000000

    return 0;
}
```

In assembly:

```asm
...
  fld dword [a]     ; Load the value of 'a' into FPU (Floating Point Unit)
  fstp dword [b]    ; Saves the converted value to 'b'
...
```


This is a different way:

```asm
...
  mov eax, 1
  cvtsi2ss xmm0, eax
...
```

The instruction **CVTSI2SS** (Convert Doubleword Integer to Scalar Single-Precision Floating-Point Value) is used in the x86 assembly to convert a 32-bit integer (doubleword integer) to a single-precision floating-point value and store it in an XMM register. Here are some details:

- Operand 1 (r/w): XMM register (like XMM0, XMM1, ecc.).
- Operand 2: general register (like EAX, ECX, EDX, ecc.) or a 32-bit memory location.

## Python
In python, explicit casting does not formally exist, it is the assignment of the result of a function to a variable:
```python
integer_number = 10
float_number = float(integer_number)
print(float_number)  # Output: 10.0
```


Another example from the official documentation https://docs.python.org/3/library/typing.html#typing.cast:
> ```python
> typing.cast(typ, val)
> ```
> Cast a value to a type.
> This returns the value unchanged. To the type checker this signals that the return value has the designated type, but at runtime we intentionally don’t check anything (we want this to be as fast as possible).
> ```python
> typing.assert_type(val, typ, /)
>```
> Ask a static type checker to confirm that val has an inferred type of `typ`.
> At runtime this does nothing: it returns the first argument unchanged with no checks or side effects, no matter the actual type of the argument.
> When a static type checker encounters a call to `assert_type()`, it emits an error if the value is not of the specified type:
> ```python
> def greet(name: str) -> None:
>    assert_type(name, str) # OK, inferred type of `name` is `str`
>    assert_type(name, int) # type checker error
>```
>
>
> This function is useful for ensuring the type checker’s understanding of a script is in line with the developer’s intentions:
> ```python
> def complex_function(arg: object):
>    # Do some complex type-narrowing logic,
>    # after which we hope the inferred type will be `int`
>    ...
>    # Test whether the type checker correctly understands our function
>    assert_type(arg, int)
> ```


## Implicit casting
### C
```C
int main() {
    int a = 5;
    float b = a / 2; // Implicit conversion to 2.0     
    printf("%f", b); // Output: 2.500000
    return 0;
}
```

### Python
```python
integer_number = 10
float_number = 3.14
sum = integer_number + float_number
print(sum)  # Output: 13.14
```
A similar behaviour occurs with the JavaScript language and its superset TypeScript, which, despite being a dynamic language, supports type declarations that are analysed at compile time before transpilation into JavaScript, but are removed because the latter is a pure dynamic language.

