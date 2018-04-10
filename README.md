# HackSkell

## What is Haskell?
Haskell is a functional programming language.

## Why use Haskell?
Haskell removes the ability to have stateful code 
because it variables are assign once. This eliminates
race conditions making it ideal for multi-threaded
applications. 

## What resources are there?
- [Learn You a Haskell for Great Good](http://learnyouahaskell.com/introduction)
- [Real World Haskell](http://book.realworldhaskell.org/read/)
- [Challenge Problems](https://wiki.haskell.org/H-99:_Ninety-Nine_Haskell_Problems)

## Some Syntax Notes

- Basic types
    - Int
    - String

- Defining a function
```
foo :: Int -> Int
foo x = <function body>
```

- Lists
    - Define a list: `let a = [4,5,6]`
    - Combine two lists: `[1,2,3] ++ [4,5,6]` yields `[1,2,3,4,5,6]`
    - Append elements:
        - Front: `3:[4,5]` yields `[3,4,5]`
        - Back: `[4,5]:6` yields `[4,5,6]`
    - List Generation `[x*x | x <- [1..10]]`
    
## Pattern Matching
Haskell provides an easy method for matching patterns in your code.
Rather than using a bunch of if-statements, you can instead define patterns in your code!

```
spellNum :: Int -> String
spellNum 1 = "One"
spellNum 2 = "Two"
spellNum 3 = "Three"
spellNum 4 = "Four"
spellNum 5 = "Five"
spellNum x = "This number is too big for my small brain"

betterSpellNum :: Int -> String
betterSpellNum num
  | num == 1  = "One"
  | num == 2  = "Two"
  | num == 3  = "Three"
  | num == 4  = "Four"
  | num == 5  = "Five"
  | otherwise = "This number is too big for my small brain"
```
    
## A Language without Loops
In Haskell there are not for or while loops, so I really 
hope you enjoy recursion. Here are some examples.

Sum a list recursively
```
printArr :: [Int] -> Int
printArr (x:xs) = x + printArr xs
printArr [] = 0
```

## Interactive TODO List
```
putTodo :: (Int, String) -> IO ()
putTodo (n, todo) = putStrLn (show n ++ ": " ++ todo)

prompt :: [String] -> IO ()
prompt todos = do
    putStrLn ""
    putStrLn "Current TODO list:"
    mapM_ putTodo (zip [0..] todos)
    command <- getLine
    interpret command todos

interpret :: String -> [String] -> IO ()
interpret ('+':' ':todo) todos = prompt (todo:todos)
interpret ('-':' ':num ) todos =
    case delete (read num) todos of
        Nothing -> do
            putStrLn "No TODO entry matches the given number"
            prompt todos
        Just todos' -> prompt todos'
interpret  "q"           todos = return ()
interpret  command       todos = do
    putStrLn ("Invalid command: `" ++ command ++ "`")
    prompt todos

delete :: Int -> [a] -> Maybe [a]
delete 0 (_:as) = Just as
delete n (a:as) = do
    as' <- delete (n - 1) as
    return (a:as')
delete _  []    = Nothing

main = do
    putStrLn "Commands:"
    putStrLn "+ <String> - Add a TODO entry"
    putStrLn "- <Int>    - Delete the numbered entry"
    putStrLn "q          - Quit"
    prompt []
```
