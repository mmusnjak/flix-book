# Monadic For-Yield

Flix also supports a _for-yield_ construct which is similar to Scala's for-comprehensions
or to Haskell's list comprehension. The _for-yield_ construct is simply syntactic sugar
for uses of `point` and `flatMap` (which requires an instance of the `Monad` type class).
The _for-yield_ construct also supports a _guard_-expression which, when used,
additionally requires an instance of the `MonadZero` type class.

The for-yield expression:

```flix
let l1 = 1 :: 2 :: Nil;
let l2 = 1 :: 2 :: Nil;
for (x <- l1; y <- l2)
    yield (x, y)
```

evaluates to the list:

```flix
(1, 1) :: (1, 2) :: (2, 1) :: (2, 2) :: Nil
```

The for-yield expression:

```flix
let l1 = 1 :: 2 :: Nil;
let l2 = 1 :: 2 :: Nil;
for (x <- l1; y <- l2; if x < y)
    yield (x, y)
```

evaluates to the list:

```flix
(1, 2) :: Nil
```

We can use for-yield on any data type which implements the `Monad` type class. For example, we can iterate through non-empty lists:

```flix
let l1 = Nel(1, 2 :: Nil);
let l2 = Nel(1, 2 :: Nil);
for (x <- l1; y <- l2)
    yield (x, y)
```

which evaluates to the non-empty list:

```flix
Nel((1, 1), (1, 2) :: (2, 1) :: (2, 2) :: Nil)
```

> **Note:** We cannot use an `if`-guard with a non-empty list because such an `if`-guard requires an instance of the `MonadZero` type class which is not implemented by non-empty list. Intuitively, we cannot use a filter in combination with a data structures that cannot be empty.