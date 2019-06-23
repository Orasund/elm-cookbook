# How can different types share data?

{% tabs %}
{% tab title="Question" %}
```text
type Animal
    = Dog DogData
    | Bird BirdData
    
type alias DogData =
    { name : String
    , runningSpeed : Float
    }
    
type alias BirdData =
    { name : String
    , wingSpan : Float
    }

{-| This function should work for all Animals
-}
getName {name} =
    name
```
{% endtab %}

{% tab title="Solution" %}
```text
type alias Animal =
    { name : String, data : AnimalData}

type AnimalData
    = Dog DogData
    | Bird BirdData
    
type alias DogData =
    { runningSpeed : Float
    }
    
type alias BirdData =
    { wingSpan : Float
    }

getName : Animal -> String
getName {name} =
    name
```
{% endtab %}

{% tab title="Alternative Solution" %}
```text
type Animal =
    Dog DogData
    | Bird BirdData

type alias AnimalData species =
    {species| name : String}
    
type alias DogData =
    AnimalData {runningSpeed:Float}

type alias BirdData =
    AnimalData {wingSpan:Float}

getName : AnimalData species -> String
getName {name} =
    name
```
{% endtab %}
{% endtabs %}

## Question

How can I write one function for all animals, that returns just the `name`.

## Answer

Put the common data, in our case the name, all in one case and separate it from the differing data.

## Further Reading

* **Thread:** [Passing accessors to functions](https://www.reddit.com/r/elm/comments/aq69vq/passing_accessors_to_functions/)



