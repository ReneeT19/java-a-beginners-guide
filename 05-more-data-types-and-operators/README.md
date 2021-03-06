# 5. More Data Types and Operators

## Arrays

- one dimensional via `type arrayName[] = new type[size];`
  - alternative syntax `type[] arrayName = new type[size];`
- initialization via `char letters[] = { 'a', 'b', 'c' };`
- out-of-bounds indexing is a runtime error.
- `java.utils.Arrays` -> `Arrays.toString(arr)` & `Arrays.deepToString(arrArr)`
- `arr.clone()`
- for multidimensional arrays, each column can have different dimensions.
- uninitialized elements are filled with default values (e.g. `0` for int, `null` for arr)
- arrays have a fixed `.length`

```java
int[][] table1 = new int[2][3]; // [[0, 0, 0], [0, 0, 0]]
int[][] table2 = new int[][] { {1, 2, 3}, {4, 5, 6} }; // [[1, 2, 3], [4, 5, 6]]
int[][] table3 = new int[][] { {1, 2, 3}, {4, 5} }; // [[1, 2, 3], [4, 5]]
```

### For-Each Loop

Can be exited early with `break`.

```java
for (int el : arr) { ... }
```

### Java Seemingly Cannot Do Statically Type-Safe Generic Arrays?

Wheeee. Spent hours today down this "fun" rabbit hole.

```java
public class Queue<T> {

    private T[] arr;

    Queue (int size) {
        // @SuppressWarnings("unchecked")
        T[] arr = (T[]) new Object[size];
        this.arr = arr;
    }
}
```

> _"The real problem is that arrays are not statically type safe. This is due to the subtype relation that states that T[] is a subtype of S[] if T is a subtype of S."_ — [JDK bug report](https://bugs.openjdk.java.net/browse/JDK-5105887?focusedCommentId=12266705&page=com.atlassian.jira.plugin.system.issuetabpanels%3Acomment-tabpanel#comment-12266705)

Relevant [Stack Overflow](https://stackoverflow.com/questions/529085/how-to-create-a-generic-array-in-java) includes a variety of (icky) answers.

- pass in class info at runtime and throw exceptions for bad values
- don't do any checking, just encapsulate as much as possible and cross your fingers
- use a different data structure, e.g. `Collection` types
- use reflection (maybe this one is ok…)

> _"All of this results from a known, and deliberate, weakness of generics in Java: it was implemented using erasure, so "generic" classes don't know what type argument they were created with at run time, and therefore can not provide type-safety unless some explicit mechanism (type-checking) is implemented."_ — [SO user Varkhan](https://stackoverflow.com/a/530289/4876305)

Ultimately, I'm going with _Effective Java_ Item 28: "Prefer Lists to Arrays."

> _"In summary, arrays and generics have very different type rules. Arrays are covariant and reified; generics are invariant and erased. As a consequence, arrays provide runtime type safety but not compile-time type safety, and vice-versa for generics. …your first impulse should be to replace the arrays with lists."_ — Effective Java

## Strings

- Java `String`s are objects.
- Can instantiate using literals, but also `new String("something")`.
- Book implies that `==` checks reference equality, but empirically two separate strings seem to `==` each other by value?
  - boxed string via `new String("str")` uses reference equality
  - string literal via `"str"` uses value equality
  - mixed strings use reference
  - use `equals` method for safety
- `String` instances are immutable, but there are also `StringBuffer` and `StringBuilder` classes which are mutable.
- `String` vals can be used in `switch` expressions.
- CLI args as `String[] args` passed into `main` by runtime

Some common methods:

- `boolean equals(str)`
- `int length()`
- `char charAt(index)`
- `int compareTo(str)` -> less than 0, greater than 0, or 0
- `int indexOf(str)` -> index or -1
- `int lastIndexOf(str)` -> index or -1
- `String substring(int start, int end)`

## Local Type Inference

- Use `var` & must include initialization
- Cannot initialize to `null`
- Cannot be a recursive definition
- Mostly useful for reference types, e.g. `var q = new Queue<Integer>(2)`
- `for (var v : nums) {...}`

## Bitwise Ops

Op    | Description
------|------------
`&`   | AND
`|`   | OR
`^`   | XOR
`~`   | NOT (one's complement)
`<<`  | Left shift
`>>`  | Right shift
`>>>` | Unsigned (zero-fill) right shift

- can be used on `long`, `int`, `short`, `char`, or `byte`
  - ops on `byte` and `short` promote val to `int`
- cannot be used on `boolean`, `float`, or `double`
- all ops can be combined with assignment in shorthand, e.g. `x ^= y`
- fun trick, `ch & 65503` turns off sixth bit – uppercasing an ASCII letter.
  - alternatively, `ch & 0b1111111111011111`
  - `ch | 32` to lowercase
- can `&` a num with different powers of two to print out the bits

## The Ternary `?` Operator

- semi-lazy in its second and third argument, like in JS
