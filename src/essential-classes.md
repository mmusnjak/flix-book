# Essential Classes

Programming in Flix invariable requires knowledge of three type classes: `Eq`,
`Order`, and `ToString`. 

### The Eq Class

The `Eq` class captures when two values of a specific type are equal:

```flix
class Eq[a] {

    pub def eq(x: a, y: a): Bool

    // ... additional members omitted ...
}
```

To implement `Eq`, we only have to implement the `eq` function.

### The Order Class

The `Order` class captures when one value is smaller or equal to another value
of the same type:

```flix
class Order[a] with Eq[a] {

    ///
    /// Returns `Comparison.LessThan` if `x` < `y`, 
    /// `Equal` if `x` == `y` or 
    /// `Comparison.GreaterThan` `if `x` > `y`.
    ///
    pub def compare(x: a, y: a): Comparison

    // ... additional members omitted ...
}
```

To implement `Order`, we only have to implement the `compare` function that
returns value of type `Comparison` which is defined as:

```flix
enum Comparison {
    case LessThan
    case EqualTo
    case GreaterThan
}
```

### The ToString Class

The `ToString` class enables us to obtain a human-readable string representation
of a value:

```flix
class ToString[a] {
    ///
    /// Returns a string representation of the given x.
    ///
    pub def toString(x: a): String
}
```

Flix uses the `ToString` type class in string interpolations. 

For example, the interpolated string

```flix
"Good morning ${name}, it is ${hour} a clock."
```

is syntactic sugar for the expression:

```flix
"Good morning " + ToString.toString(name) + ", it is " 
                + ToString.toString(hour) + " a clock."
```

In the following subsection, we discuss how implementations of the `Eq`,
`Order`, and `ToString` type classes can be automatically derived. 