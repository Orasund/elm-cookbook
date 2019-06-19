# Debounced Validation

{% tabs %}
{% tab title="Problem" %}
```text
type alias Model =
    { password : String }

type Msg =
    PasswordEntered String

{|- should only start validating if the player has not typed for 500 ms
-}
update : Msg -> Model -> (Model, Cmd Msg)
update msg =
    case msg of
        NameEntered pass =
            { model |> validate pass, Cmd.none }
```
{% endtab %}

{% tab title="Solution" %}
```
type alias Model =
    { password : String
    , debouncing : Maybe ( Field, Float )
    }
    
type Msg =
    PasswordEntered String
    | TimePassed
    
update : Msg -> Model -> (Model, Cmd Msg)
update msg =
    case msg of
        NameEntered pass =
            { model | debouncing = Just ( pass, 500 ) }
        TimePassed ->
            case debouncing of
                Just ( pass, secsLeft ) ->
                    if secsLeft <= 0 then
                        ( { model | debouncing = Nothing }
                            |> validate pass
                        , Cmd.none)

                    else
                        ( { model
                            | debouncing =
                                Just
                                    ( pass, secsLeft - checkEveryMs )
                          }
                        , Cmd.none
                        }

            
subscriptions : Model -> Sub Msg
subscriptions { debouncing } =
    case debouncing of
        Just _ ->
            Time.every 100 (always TimePassed)

        Nothing ->
            Sub.none
```
{% endtab %}
{% endtabs %}

## Question

How can I validate the password only if the user has not typed for 500ms?

## Answer

Subscribe to time passing, based on whether a password need to be debounced or not. Start counting down the ms that have passed and then update.

## Further reading

* **Thread:** [Example: Debounced Validation](https://discourse.elm-lang.org/t/example-debounced-validation/3804)

