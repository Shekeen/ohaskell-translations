### Tuples ###

A tuple is a special kind of a list. It also stores a set of elements, but has three fundamental differences from a list:

* parentheses instead of brackets
* geterogeneity
* type depending on size

The first difference is obvious:

```haskell
["Denis", "Shevchenko"]  -- this is a list of two strings
("Denis", "Shevchenko")  -- this is a tuple of two strings
```

The second difference is the tuple's ability to store elements of different types:

```haskell
["Denis", 1.234]  -- this won't compile, I assure you
("Denis", 1.234)  -- but this will
```

Now the third difference. If we have two lists of strings that differ in size:

```haskell
["Denis", "Vasil`evich", "Shevchenko"]
["Denis", "Shevchenko"]
```

the type of both these lists is the same, namely `[String]`. The list's type doesn't depend on the number of the elements it has.

The tuples are different. If we have two tuples that differ in length:

```haskell
("Denis", "Vasil`evich", "Shevchenko")
("Denis", "Shevchenko")
```

the types of these tuples are different: the first one's is `(String, String, String)`, the second one's is `(String, String)`. Therefore if you apply a function which expects a tuple of two strings as an argument to a tuple of three strings, the compiler will protest:

```
Couldn't match expected type `(String, String)'
            with actual type `([Char], [Char], [Char])'
```

That's understandable: we were expecting a tuple of two strings, but suddenly received a tuple of three!

By the way, the tuple is also similar to a list in a way that it can also be empty, i.e. contain no elements.

#### What can you do to them ####

The only thing you can do to a tuple - extract its elements. That's it.

Two-element tuples are most common in practice. These tuples are also called a pair. To extract its elements, we have standard functions `fst` and `snd`.

Let's define a function, which is applied to a tuple containing two parts of a chess move:

```haskell
chessMove :: (String, String) -> String
chessMove pair = fst pair ++ "-" ++ snd pair

main = print $ chessMove ("e2", "e4")
```

We consequently extracted the first and the second elements from the given pair and made a single string from them.

But what should we do, if there are more than two elements in the tuple? `fst` and `snd` functions work only with pairs. If the number of elements is greater than two, we should extract them differently.

#### Inconvenient way ####

The first way is inconvenient, as we need to define the necessary functions ourselves. But we are not afraid of difficulties, so let's do it:

```haskell
get1 (element, _, _, _) = element
get2 (_, element, _, _) = element
get3 (_, _, element, _) = element
get4 (_, _, _, element) = element
```

We assume that we want to work with tuples of four elements. In this case we have only four cases of extraction, so we define functions to extract the first, the second, the third, and the fourth elements. By the way, when we say "the first element" we don't mean the index, the digit `1` is a sequence number.

Now let's consider the first function's definition:

```haskell
get1 (element, _, _, _) = element
```

This function is applied to a tuple of four elements and returns the first one of them. Pay attention to those strange underscore symbols. Think about those as "something", or "whatever it is". We say: "Yes, this tuple has four elements, but we don't care, what is under number two, or number three, or number four. The one thing we care about is number one. And it we shall return."

The same is with the second function:

```haskell
get2 (_, element, _, _) = element
```

We get four elements, but we don't care about numbers one, three, and four, we need just the number two, and we return it.

And now we write:

```haskell
main = print $ get3 ("One", "Two", "Three", "Four")
```

and we get the expected result

```
Three
```

#### Convenient way ####

Why do something if it's already done by others? And others made the `tuple` package from Hackage.

