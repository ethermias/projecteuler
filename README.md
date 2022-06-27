# projecteuler
Haskell Solution to Project Euler
## Problem 1
```
If we list all the natural numbers below 10 that are multiples of 3 or 5, we get 3, 5, 6 and 9. The sum of these multiples is 23.

Find the sum of all the multiples of 3 or 5 below 1000.
```
### Answer 1
```
sum [x | x <- [1..999], x `mod` 3 == 0 || x `mod` 5 == 0]
```
## Problem 2

Each new term in the Fibonacci sequence is generated by adding the previous two terms. By starting with 1 and 2, the first 10 terms will be:

1, 2, 3, 5, 8, 13, 21, 34, 55, 89, ...

By considering the terms in the Fibonacci sequence whose values do not exceed four million, find the sum of the even-valued terms.

### Answer 2
```
fab :: Int -> Int
fab 1 = 1
fab 2 = 2
fab n = (fab (n-1)) + (fab (n-2))

fabList y = sum $ filter (\n -> n `mod` 2 == 0) $ takeWhile (<y) $ map fab [1..]
```
## Problem 3
```
The prime factors of 13195 are 5, 7, 13 and 29.

What is the largest prime factor of the number 600851475143 ?
```

### Answer 3

```
isDiv :: Integer -> Integer -> Integer
isDiv y n
    | y `mod` n == 0 =  0
    | otherwise = n

step0 x y = last $ takeWhile (\z ->  z /= 0 ) $ map f [x..y] where f = isDiv y

step1 :: Integer -> Integer
step1 y = 1 + (step0 2 y) 

result :: Integer -> Integer
result x
      | mm  > 1 = result mm 
      | otherwise  = x
      where mm = x `div` (step1 x)
```

## Problem 4
```
A palindromic number reads the same both ways. The largest palindrome made from the product of two 2-digit numbers is 9009 = 91 × 99.

Find the largest palindrome made from the product of two 3-digit numbers.
```
### Answer 4

```
maxNum :: Ord a => [a] -> a
maxNum [x] = x
maxNum (x:x':xs) = maxNum ((if x >= x' then x else x'):xs)

reverseInt = read . reverse . show
next n  = take 1 $ dropWhile (\z ->  z /= reverseInt z ) $ map (*n) [n, n-1..1]
min2 m = concat (map next [m, m-1..1])
```

## Problem 5
```
2520 is the smallest number that can be divided by each of the numbers from 1 to 10 without any remainder.

What is the smallest positive number that is evenly divisible by all of the numbers from 1 to 20?
```
### Answer 5
```
import Data.List
prime_factors n =
  case factors of
    [] -> [n]
    _  -> factors ++ prime_factors (n `div` (head factors))
  where factors = take 1 $ filter (\x -> (n `mod` x) == 0) [2 .. n-1]

all0 = map prime_factors [2..20]
all1 xs ys = intersect xs ys
all2 xs ys  = ys \\ (all1 xs ys)
all3 = foldl (\x acc -> (all2 acc x) ++ acc) [] all0
result = foldl (\x acc -> x * acc ) 1 all3
```
