# Restrict Records using Opaque Types

{% hint style="info" %}
If you're writing a package use a config pipe API instead:

* `new : Movie`
* `withTitle : String -> Movie -> Movie`
* `withRating : Int -> Movie -> Movie`
* `rate : Int -> Movie -> Maybe Movie`
{% endhint %}

{% tabs %}
{% tab title="Problem" %}
{% code-tabs %}
{% code-tabs-item title="Main.elm" %}
```text
type alias Movie = 
    { title : String
    , rating : Int
    }

{-| adds a Rating to a Movie,
Only allows ratings between 1 and 5.
-}     
addRating : Int -> Movie -> Movie
addRating rating movie =
    { movie | rating = rating}
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}

{% tab title="Solution" %}
{% code-tabs %}
{% code-tabs-item title="Movie.elm" %}
```text
module Movie exposing (Movie,fromTitle,addRating)

type Movie =
    Movie 
        { title : String
        , rating : Int
        }

{-| adds a Rating to a Movie,
Only allows ratings between 1 and 5.
-}     
addRating : Int -> Movie -> Maybe Movie
addRating rating (Movie movie) =
    if 1 <= rating && rating <= 5 then 
        Just <| Movie { movie | rating = rating}
    else
        Nothing

{-| Constructs a Movie from a Title
-}
fromTitle : String -> Movie
fromTitle title =
  Movie
    { title = title
    , rating = 0
    }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="Main.elm" %}
```text
import Movie exposing (Movie)

newMovie : String -> Int -> Movie
newMovie title rating =
    let
        movie = Movie.fromTitle title
    in
    movie
    |> Movie.addRating rating
    |> Maybe.withDefault movie
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}

## Question

How can I only allow specific states for my type?

## Answer

Place the type into a new File and don't expose the constructor: `exposing (Movie)` instead of `exposing(Movie(..))`.  
Then write your own constructors.

{% hint style="info" %}
We can unwrap function parameters like this:

```text
addRating rating (Movie movie) =
    ...
```
{% endhint %}

## Further reading

* **Article:** [Advanced Types in Elm - Opaque Types](https://medium.com/@ckoster22/advanced-types-in-elm-opaque-types-ec5ec3b84ed2) by Charlie Koster

