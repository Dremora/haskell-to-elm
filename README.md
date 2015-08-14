# haskell-to-elm

Collection of examples on places where Elm is different to Haskell.

Used for helping beginners moving from Haskell to Elm. Non-exhaustive list, only to be used alongside the documentation on the [Elm](elm-lang.org) site.

* [Functional Programming](#functional-programming)
	* [Type signatures](#type-signatures)
	* [Function application](#function-application)
	* [Function composition](#function-composition)
	* [List comprehensions](#list-comprenhensions)
	* [Lenses](#lenses)
	* [Where](#where-vs-let)
* [Prelude functions](#built-in-prelude-methods)
	* [id](#id)
	* [cons](#cons)
	* [zip](#zip)
	* [show](#show)
	* [mod](#mod)
	* [cycle](#cycle)
* [Modules](#module-syntax)
	* [Importing](#importing-names)
	* [Exporting](#defining-exportable-names)

# Functional programming 

## Type signatures

Elm uses a [single colon `(:)`](http://elm-lang.org/docs/syntax#type-annotations) for type signatures. Double colons `(::)` is used for [cons](http://package.elm-lang.org/packages/elm-lang/core/2.1.0/List#::).

Example

```
	add :: Int -> Int
```

becomes
```
	add : Int -> Int
```

## Function application

Instead of using the dollar symbol `$` elm uses `<|` and `|>`  for [application in different directions](http://package.elm-lang.org/packages/elm-lang/core/2.1.0/Basics#|>). 

Example:

```
    collage (round board.width) (round board.height) $ map (genRect board) board.pieces
```
becomes
```
    collage (round board.width) (round board.height) <| List.map (genRect board) board.pieces
```

## Function composition

Instead of using the `(.)` symbol Elm uses `<<` and `>>` for [composition in different directions](http://package.elm-lang.org/packages/elm-lang/core/2.1.0/Basics#>>)

Example:
```
    isEvenSquareRoot = sqrt . isEven
```
becomes
```
    isEvenSquareRoot = sqrt << isEven
    -- or
    isEvenSquareRoot = isEven >> sqrt
```

## List comprenhensions

There are [no list comprehensions](https://github.com/elm-lang/elm-compiler/issues/147#issuecomment-17439271) in Elm.

## Lenses

Elm has the package [focus](http://package.elm-lang.org/packages/evancz/focus/1.0.1) for lense-like accessors. Due to a lack of template-haskell like functionality, you must always [manually create your own focus](http://package.elm-lang.org/packages/evancz/focus/1.0.1/Focus#create)



Example:
``` 
    data Patch = Patch {
		_colour :: Colour,
		_size :: Double, 
		_coord :: Coordinate
	} deriving (Show, Eq, Ord)
	
	mkLabels[''Patch]
```
becomes
```
    type alias Patch = {
        colour: Colour,
        size: Float,
        coord: Coordinate
    }
    
    colour = create .colour (\f r -> { r | colour <- f r.colour })
    coord = create .coord (\f r -> { r | coord <- f r.coord })
    size = create .size (\f r -> { r | size <- f r.size })
```

## where vs let

Elm has no where binding - instead use [let](http://elm-lang.org/docs/syntax#let-expressions)

# Built-in (Prelude) methods

## id

Elm has renamed id to [identity](http://package.elm-lang.org/packages/elm-lang/core/2.1.0/Basics#identity)

Example:
```
	id xs
```
becomes
```
	identity xs
```

## cons

Elm uses [double colons `(::)`](http://package.elm-lang.org/packages/elm-lang/core/2.1.0/List#::) for cons.

Example
```
	5 : 6 : [7, 8]
```
becomes
```
	5 :: 6 :: 7 :: [7. 8]
```

## zip

Elm has [no built-in zip](http://elm-lang.org/examples/zip) method - instead it provides a [map2](http://package.elm-lang.org/packages/elm-lang/core/2.1.0/List#map2) function that can be used with the tuple creator `(,)` to make a list of size 2 tuples from two lists.

Example:

```
    zip xs ys
```
becomes
```
    map2 (,) xs ys
```


## show

Elm renamed [show](http://zvon.org/other/haskell/Outputprelude/show_f.html) to [toString](http://package.elm-lang.org/packages/elm-lang/core/2.1.0/Basics#toString). Confusingly, there is also a [method called show](http://package.elm-lang.org/packages/elm-lang/core/2.1.0/Graphics-Element#show) in Elm - this generates a HTML element containing a textual representation of the data.

Example:
```
    show [1, 2, 3]
```
becomes
```
    toString [1, 2, 3]
```

## mod

mod in Elm uses the [`(%)` symbol](http://package.elm-lang.org/packages/elm-lang/core/2.1.0/Basics#%).

Example:
```
    isEven x = x `mod` 2 == 0
```
becomes
```
    isEven x = x % 2 == 0
```

## unwords

unwords is replaced by the [join function](http://package.elm-lang.org/packages/elm-lang/core/2.1.0/String#join)

Example:
```
	unwords $ words "Hello Dave and Jeremy"
```
becomes
```
	join " " <| words "Hello Dave and Jeremy"
```

## cycle

Elm has no cycle built in. 


TODO: find documentation for this

# Module syntax

## Importing names

Elm uses the [`exposing` keyword](http://elm-lang.org/docs/syntax#modules) to import names into the current namespace.

Example:
```
    import List (map, foldl)
```
becomes
```
    import List exposing (map, foldl)
```

## Defining exportable names

TODO: find documentation on elm site for this

Following the module declaration, you must have *no* identnation level.

Example:
```
    module Coords (pos) where
        pos x y = (x, y)
```
becomes
```
    module Coords (pos) where
    pos x y = (x, y)
```
