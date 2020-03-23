# Reusable views

{% tabs %}
{% tab title="Problem" %}
{% code title="Main.elm " %}
```
type alias ButtonModel =
    { text : String
    }

type alias Model =
    { button: ButtonModel
    }

type ButtonMsg =
    ButtonPressed

type Msg =
    ButtonSpecific ButtonMsg
    | Reset

update : Msg -> Model -> (Model,Cmd Msg)
update msg ({button} as model) =
    case msg of
        ButtonSpecific ButtonPressed -> 
            ( { model
              | button = 
                  { button
                  | text = "Thanks!"
                  }
              }
            , Cmd.none
            ) 
        _ ->
            (model, Cmd.none)

{-|
  This can be moved into a seperate file as it still 
  depends on Msg.
-}
myButton : ButtonModel -> List (Attribute Msg) -> List (Html Msg) -> Html Msg
myButton {text} attributes children =
    Html.div []
        (Html.button
            [ (Events.onClick <| ButtonSpecific ButtonPressed)
              :: attributes 
            ]
            [ Html.text text
            ]
        )
        :: children

view : Model -> Html Msg
view model =
    Html.div []
        [ Html.h1 []
            [ Html.text "Here is a reusable view for a button"
            ]
        , myButton []
            [ Html.text "Click me!"
            ]
        ]
```
{% endcode %}
{% endtab %}

{% tab title="Solution" %}
{% code title="Main.elm" %}
```text
import MyButton

type alias Model =
    { button: MyButton.Model
    }

type Msg =
    | ButtonPressed String
    | Reset

update : Msg -> Model -> (Model,Cmd Msg)
update msg ({button} as model) =
    case msg of
        ButtonPressed string -> 
            ( button |> MyButton.buttonPressed string
            , cmd
            ) 
        _ ->
            (model, Cmd.none)

view : Model -> Html Msg
view ({button} as model) =
    Html.div []
        [ Html.h1 []
            [ Html.text "Here is a reusable view for a button"
            ]
        , MyButton.view ButtonPressed
            button
            []
            [ Html.text "Click me!"
            ]
        ]
```
{% endcode %}

{% code title="MyButton.elm" %}
```
type alias Model =
    { text : String
    }

buttonPressed : String -> Model -> Model
buttonPressed string button =
  { button
  | text = string
  }

view : (string -> msg) -> ButtonModel -> List (Attribute msg) -> List (Html msg) -> Html msg
view onClick {text} attributes children =
    Html.div []
        (Html.button
            [ (Events.onClick <| onClick <| "Thanks!")
              :: attributes 
            ]
            [ Html.text text
            ]
        )
        :: children
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Question

How can I build a reusable view function with its own model?

## Answer

The trick is to pass the Messages along. 

## Further reading

* 📄**Article:** [rofrol/elm-best-practices.md](https://gist.github.com/rofrol/fd46e9570728193fddcc234094a0bd99#reusable-views-instead-of-nested-components)
* 📄**Article:** [Render props for Elm](https://hackernoon.com/render-props-for-elm-d5547efd66f5) by Boolean Julian Jelfs
* ❗**Example:** [NoRedInk/elm-sortable-table](https://package.elm-lang.org/packages/NoRedInk/elm-sortable-table/latest/)
* ❗**Example:** [abadi199/datetimepicker](https://package.elm-lang.org/packages/abadi199/datetimepicker/latest/DateTimePicker)
* ❗**Example:** [ContaSystemer/elm-menu](https://package.elm-lang.org/packages/ContaSystemer/elm-menu/latest/)

