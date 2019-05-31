# Why are Booleans bad?



{% tabs %}
{% tab title="Problem" %}
```text
type alias Request =
    { fetching : Bool
    , error : String
    , message : String
    }

getResponse : Request -> ( String, Bool )
getResponse request =
    if request.fetching then
        ( "", True)
    else if error == "" then
        ( request.message, True )
    else
        ( request.error, False )
```
{% endtab %}

{% tab title="Solution" %}
```text
type Request =
    Fetching
    | Error String
    | Message String

getResponse : Request -> Maybe (Result String String)
getResponse request =
    case request of
        Fetching ->
            Nothing
        Error error ->
            Just <| Err error
        Ok message ->
            Just <| Ok message
```
{% endtab %}
{% endtabs %}

## Question

I heard that Booleans should be avoided in Elm, How and Why?

## Answer

Booleans create a lot of problems, for example what does it mean if `Request.fetching` is `False` but `Request.message` has some value? Or how can one know what the returned boolean of `getResponse` stands for? The type system of Elm can avoid such problems:

* Use `Maybe (String, Bool)` instead of returning some default value.
* Use `Result String String` to handle results
* Use a Custom Type and Patter Matching instead of `Request.fetching` and If-Statements.

## Further reading

* **Video:** [Solving the Boolean Identity Crisis](https://www.youtube.com/watch?v=8Af1bh-BVY8) by Jeremy Fairbank
* **Blog:** [Solving the Boolean Identity Crisis Part 1](https://programming-elm.com/blog/2019-05-20-solving-the-boolean-identity-crisis-part-1/) by Jeremy Fairbank
* **Blog:** [Solving the Boolean Identity Crisis Part 2](https://programming-elm.com/blog/2019-05-30-solving-the-boolean-identity-crisis-part-2/) by Jeremy Fairbank

