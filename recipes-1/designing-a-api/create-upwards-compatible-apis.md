# Create upwards compatible APIs

{% tabs %}
{% tab title="Problem" %}
{% code-tabs %}
{% code-tabs-item title="Movie.elm" %}
```text
type Movie =
    Movie 
        { title : String
        , rating : Int
        , -- adding a field will destroy the function below
        }

new : String -> Int -> Movie
new title rating =
  Movie
    { title = title
    , rating = clamp 1 5 rating
    }
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}

{% tab title="Solution" %}
{% code-tabs %}
{% code-tabs-item title="Movie.elm" %}
```text
type Movie =
    Movie 
        { title : String
        , rating : Int
        , director : Maybe String
        }

fromTitle : String -> Movie -> Movie
fromTitle title =
    Movie 
        { title : title
        , rating : 0
        , director : Nothing
        }

withRating : Int -> Movie -> Movie
withRating rating (Movie movie) =
    { movie | rating = clamp 1 5 rating }

withDirector : String -> Movie -> Movie
withDirector director (Movie movie) =
    { movie | director = director }
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}

## Question

How can I define my type, such that I can add features without breaking the API?

## Answer

First write a constructor `fromTitle` that only uses as few arguments as possible.  
Next add partial constructors for every feature of your type: `withRating`  and `withDirector`.

Now creating a new `Movie` can be done like this:

```text
Movie.fromTitle "Life of Brian"
|> Movie.withDirector "Terry Jones"
|> Movie.withRating 5
```

## Further Reading

* **Video:** [Robot Buttons from Mars](https://www.youtube.com/watch?v=PDyWP-0H4Zo) by Brian Hicks
* **Example:** [NoRedInk/elm-json-decode-pipeline](https://package.elm-lang.org/packages/NoRedInk/elm-json-decode-pipeline/latest/)
* **Example:** [Chadtech/random-pipeline](https://package.elm-lang.org/packages/Chadtech/random-pipeline/latest/)
* **Example:** [Particle Type](https://package.elm-lang.org/packages/BrianHicks/elm-particle/latest/Particle#Particle) from BrianHicks/elm-particle
* **Example:** [Request Type](https://package.elm-lang.org/packages/dillonkearns/elm-graphql/latest/Graphql-Http#withHeader) from dillonkearns/elm-graph
* **Example:** [Image Type](https://package.elm-lang.org/packages/Orasund/pixelengine/latest/PixelEngine-Image#Image) from Orasund/pixelengine

