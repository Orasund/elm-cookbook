# Reusable views

{% tabs %}
{% tab title="Problem" %}
```
check out Main.elm for the full code.
```

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

{-| This can be moved into a seperate file as it still depends on Msg.
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
{% endtab %}

{% tab title="Solution" %}
```text
exposing MyButton

type alias Model =
    { button: MyButton.Model
    }

type Msg =
    ButtonSpecific MyButton.Msg
    | Reset

update : Msg -> Model -> (Model,Cmd Msg)
update msg ({button} as model) =
    case msg of
        ButtonSpecific buttonMsg -> 
            let
                ( newButton, cmd) =
                    MyButton.update
                        ButtonSpecific
                        buttonMsg
                        button
            in
            ( { button = newButton
              }
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
        , myButton ButtonSpecific
            button
            []
            [ Html.text "Click me!"
            ]
        ]
```

```
type alias ButtonModel =
    { text : String
    }

type ButtonMsg =
    ButtonPressed

update : (ButtonMsg -> msg) -> ButtonMsg -> ButtonModel -> (ButtonModel,Cmd msg)
update msgWrapper msg ({button} as model) =
    case msg of
        Button.Pressed -> 
            ( { button
              | text = "Thanks!"
              }
            , Cmd.none |> Cmd.map msgWrapper 
            ) 
        _ ->
            (model, Cmd.none |> Cmd.map msgWrapper)

myButton : (ButtonMsg -> msg) -> ButtonModel -> List (Attribute msg) -> List (Html msg) -> Html msg
myButton msgWrapper {text} attributes children =
    Html.div []
        (Html.button
            [ (Events.onClick <| msgWrapper ButtonPressed)
              :: attributes 
            ]
            [ Html.text text
            ]
        )
        :: children
```
{% endtab %}
{% endtabs %}

## Question

How can I build a reusable view function with its own model and update function?

## Answer

The trick is to add an argument `(ButtonMsg -> msg)`. 

## Further reading

* **Article:** [Render props for Elm](https://hackernoon.com/render-props-for-elm-d5547efd66f5) by Boolean Julian Jelfs
* **Example:** [NoRedInk/elm-sortable-table](https://package.elm-lang.org/packages/NoRedInk/elm-sortable-table/latest/)
* **Example:** [abadi199/datetimepicker](https://package.elm-lang.org/packages/abadi199/datetimepicker/latest/DateTimePicker)
* **Example:** [ContaSystemer/elm-menu](https://package.elm-lang.org/packages/ContaSystemer/elm-menu/latest/)

