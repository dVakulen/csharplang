# Target-typed "default" literal

* [x] Proposed
* [ ] Prototype
* [ ] Implementation
* [ ] Specification

## Summary
[summary]: #summary

The target-typed `default` feature is a shorter form variation of the `default(T)` operator, which allows the type to be omitted. Its type is inferred by target-typing instead. Aside from that, it behaves like `default(T)`.

## Motivation
[motivation]: #motivation

The main motivation is to avoid typing redundant information.

For instance, when invoking `void Method(ImmutableArray<SomeType> array)`, the *default* literal allows `M(default)` in place of `M(default(ImmutableArray<SomeType>))`.

This is applicable in a number of scenarios, such as:

- declaring locals (`ImmutableArray<SomeType> x = default;`)
- ternary operations (`var x = flag ? default : ImmutableArray<SomeType>.Empty;`)
- returning in methods and lambdas (`return default;`)
- declaring default values for optional parameters (`void Method(ImmutableArray<SomeType> arrayOpt = default)`)
- including default values in array creation expressions (`var x = new[] { default, ImmutableArray.Create(y) };`)


## Detailed design
[design]: #detailed-design

A new expression is introduced, the *default* literal. An expression with this classification can be implicitly converted to any type, by a *default-or-null literal conversion*. 

The inference of the type for the *default* literal works the same as that for the *null* literal, except that any type is allowed (not just reference types).

This conversion produces the default value of the inferred type.

The *default* literal may have a constant value, depending on the inferred type. So `const int x = default;` is legal, but `const int? y = default;` is not.

The *default* literal can be the operand of equality operators, as long as the other operand has a type. So `default == x` and `x == default` are valid expressions, but `default == default` is illegal.

## Drawbacks
[drawbacks]: #drawbacks

A minor drawback is that *default* literal can be used in place of *null* literal in most contexts. Two of the exceptions are `throw null;` and `null == null`, which are allowed for the *null* literal, but not the *default* literal.

## Alternatives
[alternatives]: #alternatives

There are a couple of alternatives to consider:

- The status quo:  The feature is not justified on its own merits and developers continue to use the default operator with an explicit type.
- Extending the null literal: This is the VB approach with `Nothing`. We could allow `int x = null;`.

## Unresolved questions
[unresolved]: #unresolved-questions

- [ ] Should *default* be allowed as the operand of the *is* or *as* operators?

## Design meetings

n/a
