# What are comparable types?

{% tabs %}
{% tab title="Problem" %}
```text
type FruitSort =
    Apple
    | Orange
    | Banana

type alias Fruit =
    { sort: FruitSort
    , name: String
    }

{-| Fruit needs to be "comparable". What do i need to do?
-}
type Basket =
    Dict Fruit Int
```
{% endtab %}

{% tab title="Solution" %}
```text
type FruitSort =
    Apple
    | Orange
    | Banana

fruitSortToInt : FruitSort -> Int
fruitSortToInt fruit =
    case fruit of
        Apple -> 1
        Orange -> 2
        Banana -> 3

fruitSortFromInt : Int -> FruitSort
fruitSortFromInt int =
   case int of
       1 -> Apple
       2 -> Orange
       3 -> Banana
       _ -> Apple --be careful, the compiler won't help you there.

type alias Fruit =
    { sort: FruitSort
    , name: String
    }

type alias Item =
    (Int,String)

itemFromFruit : Fruit -> Item
itemFromFruit {sort,name} =
    (sort |> fruitSortToInt,name)

itemToFruit : Item -> Fruit
itemToFruit (int,name) =
    { sort = int |> fruitSortFromInt
    , name = name
    }

type Basket =
    Dict Item Int
```
{% endtab %}
{% endtabs %}

## Question

`Dict` needs the first type to be comparable. What does that mean?

## Answer

`Int`, `Float`, `Char`, `String`, `Tuple comparable comparable` and `List comparable` are `comparable` types.  
  
So you need to convert Fruit into a comparable type like `Tuple Int String`.

## Further reading

* 📦**Package:** Use any type with [turboMaCk/any-dict](https://package.elm-lang.org/packages/turboMaCk/any-dict/latest/).
* 📦**Package:** use any type with [turboMaCk/any-set](https://package.elm-lang.org/packages/turboMaCk/any-set/latest/).

