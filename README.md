almost-fix
==========

Combinators for predicative recursion

## Usage

### Simple Predicative Recursion

Say you want to perform `f` until some boolean test `b`:

```haskell
almostFix b f
```

will run `f` until `b` returns `False`.

```haskell
almostFix True f  ~  f . f . f ...
```

### Monadic Predicates

Say you've got a _monadic_ step `f :: a -> m a`, and some boolean test in the
monad `b :: m Bool` - a good example of this would be in a `MonadState Integer m`
stateful monad, and the monadic predicate `liftM (< 5) get :: m Bool`. Now, we
_bind_ until the predicate is falsified:

```haskell
exclaimAgain :: MonadState Integer m => String -> m String
exclaimAgain a = do modify (+1)
                    return (a ++ "!")

exclaimFive :: String -> String
exclaimFive s = evalState (almostFix (liftM (<= 5) get) exclaimAgain s) 1
```
