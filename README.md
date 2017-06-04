# teach-haskelll

## Codes

Short solution for pangram:

```haskell
import Data.Char (toLower)

alphabet :: String
alphabet =
  ['a'..'z']

isPangram :: String -> Bool
isPangram text =
  all (isIn normed) alphabet
  where
    normed = fmap toLower text
    isIn = flip elem
```

Advanced (ie over-engineered 🙃) solution to leap:

```haskell
import Control.Applicative (liftA2)

type PredicateCombinator a = (a -> Bool) -> (a -> Bool) -> a -> Bool

makePredicateCombinator :: (Bool -> Bool -> Bool) -> PredicateCombinator a
makePredicateCombinator =
  liftA2

(&&->) :: PredicateCombinator a
(&&->) =
  makePredicateCombinator (&&)

(||->) :: PredicateCombinator a
(||->) =
  makePredicateCombinator (||)

isLeapYear :: Integer -> Bool
isLeapYear =
  isDivisibleBy4 &&-> (isNotDivisibleBy100 ||-> isDivisibleBy400)
  where
    isDivisibleBy4 = (==0) . flip mod 4
    isNotDivisibleBy100 = (/=0) . flip mod 100
    isDivisibleBy400 = (==0) . flip mod 400
```

Etl:

```haskell
module ETL (transform) where

import Data.Map
  (Map
  ,singleton
  ,foldMapWithKey)

import Data.Char
  (toLower)

transform :: Map a String -> Map Char a
transform =
  foldMapWithKey invertMap
  where
    invertMap k = foldMap (flip singleton k . toLower)
```

Discussion:

* Hoogled for `Map k [v] -> Map v k` and found nothing
* Saw `Map` has `Monoid` instance `Ord k => Monoid (Map k v)`
* `Char` has `Ord` instance so the inverse `Map` has `Monoid`
* Idea is to take `singleton :: k -> v -> Map k v` and form maps for each `v` in `[v]` combining them with `mconcat`
* Use `foldMap :: Monoid m => (a -> m) -> [a] -> m` to do this over `[v]`
* `Map` provides perfect function to combine all inverted maps `foldMapWithKey :: Monoid m => (k -> a -> m) -> Map k a -> m`
* ie `(k -> [v] -> Map v k) -> Map k [v] -> Map v k`

## Pro-tips
* [Hoogle](https://www.stackage.org/lts/hoogle) is your friend, and it's better on stackage
* [Intero](https://commercialhaskell.github.io/intero/) is another friend
* [Spacemacs](http://spacemacs.org/) may be your best friend
* [Doctest](https://github.com/sol/doctest#readme) bonus

## Todo

- [ ] exercise for next lunch: http://exercism.io/exercises/haskell/etl/readme
