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

## Pro-tips
* [Hoogle](https://www.haskell.org/hoogle/) is your friend
* [Intero](https://commercialhaskell.github.io/intero/) is another friend
* [Spacemacs](http://spacemacs.org/) may be your best friend
* [Doctest](https://github.com/sol/doctest#readme) bonus
