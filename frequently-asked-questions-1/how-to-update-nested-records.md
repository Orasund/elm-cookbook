# How to update nested Records?

{% hint style="warning" %}
**Not Good Practice:** Create an [opaque type](../recipes-1/making-impossible-states-impossible/restrict-records-using-opaque-types.md) instead.
{% endhint %}

{% tabs %}
{% tab title="Problem" %}
```text
type alias Movie = 
    { title : String
    , rating : Int
    }
  
type alias Model = 
    { currentMovie : Movie
    }

rate : Int -> Model -> Model
rate newRating model =
    { model 
    | currentMovie = 
        --{ model.currentMovie | rating = newRating }
        Debug.todo "the line above does not compile"
    }
```
{% endtab %}

{% tab title="Solution" %}
```text
type alias Movie = 
    { title : String
    , rating : Int
    }
  
type alias Model = 
    { currentMovie : Movie
    }

rate : Int -> Model -> Model
rate newRating ({currentMovie} as model) =
    { model 
    | currentMovie = 
        {currentMovie | rating = newRating }
    }
```
{% endtab %}

{% tab title="Alternative Solution" %}
```text
type alias Movie = 
    { title : String
    , rating : Int
    }
  
type alias Model = 
    { currentMovie : Movie
    }

rate : Int -> Model -> Model
rate rating ({currentMovie} as model) =
    model
    |> updateMovie (addRating rating)

updateMovie : (Movie -> Movie) -> Model -> Model
updateMovie fun ({currentMovie} as model) =
    { model | currentMovie = fun currentMovie}
   
addRating : Int -> Movie -> Movie
addRating rating movie =
    { movie | rating = rating}
```

{% hint style="success" %}
**This is the a better solution**
{% endhint %}
{% endtab %}
{% endtabs %}

## Question

How can I update a nested record field?

## Answer

First get the nested record, then update it:

```text
let
    currentMovie = model.currentMovie
in
{ currentMovie | rating = newRating}
```

{% hint style="info" %}
For function parameters we can use the keyword `as` to bind fields of a record to a variable with the same name:

```text
rate newRating ({currentMovie} as model) =
    ...
```
{% endhint %}

## Further reading

* **Article:** [Updating nested records in Elm](https://medium.com/elm-shorts/updating-nested-records-in-elm-15d162e80480) by Wouter In t Velt

