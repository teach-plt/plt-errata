# plt-errata
Collection of errata for book Aarne Ranta, Implementing Programming Languages
http://www1.digitalgrammars.com/ipl-book/

To add a new erratum, create an issue, and use github markdown syntax and adhere to the style of this page.
I will then the erratum below.

## Known errata

This includes the errata listed on http://www1.digitalgrammars.com/ipl-book/

### Chapter 1, Compilation Phases

p. 10 (and also later): it is stated that Python is an untyped language. By this we mean that Python has no compile-time type checking. But it does have a run-time notion of types, known as dynamic typing.

### Chapter 2, Grammars

p. 27: too many classes in the Java example have the name `EAdd`. Should be `EAdd`, `ESub`, `EMul`, `EDiv`.

### Chapter 4, Type Checking

#### 4.10 Annotating type checkers

p. 69: the pseudo-code for `infer(Γ,a+b)` is wrong in that it removes the annotations from the subexpressions of the addition expression.  The correct return statement would be
```
  return [['a : t] + ['b : t] : t]
```

p. 70: the pseudo-code for `infer(Γ,a+b)` has the same problem as on p. 69
#### 4.11 Type checker in Haskell

p. 73: in `checkExp` code, `if (typ2 = typ)` should be `if typ2 == typ`.
It could also be written as
```haskell
  unless (typ2 == typ) $ fail $
    "type of " ++ ...
```
In `checkStm`, there are several errors.  The correct code is:
```haskell
checkStm :: Env -> Stm -> Err Env
checkStm env s = case s of
  SExp exp -> do
    inferExp env exp
    return env
  SDecl typ x ->
    updateVar env x typ
  SWhile exp stm -> do
    checkExp env Type_bool exp
    checkStm env stm
```

### Chapter 5, Interpreters

p. 92: in the last rule for `ifeq L`, the code pointer should become `P+1` when `v != 0`

### Chapter 8, The language design space

p. 170: the `lin` rules for `TAll` and `TAny` generate a bogus condition. The proper rules are:
```
    TAll kind = parenth ("\\p -> and [p x | x <-" ++ kind ++ "]") ;
    TAny kind = parenth ("\\p -> or  [p x | x <-" ++ kind ++ "]") ;
```
