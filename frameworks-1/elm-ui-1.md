# mdgriffith/elm-ui

> CSS and HTML are actually quite difficult to use when you're trying to do the layout and styling of a web page.
>
> This library is a complete alternative to HTML and CSS. Basically you can just write your app using this library and \(mostly\) never have to think about HTML and CSS again.
>
> The high level goal of this library is to be a **design toolkit** that draws inspiration from the domains of design, layout, and typography, as opposed to drawing from the ideas as implemented in CSS and HTML.
>
> \(Readme.md from the [package](https://package.elm-lang.org/packages/mdgriffith/elm-ui/1.1.0/)\)

```text
main : Html msg
main = 
    Element.layout [] <|
        row [ width fill, centerY, spacing 30 ]
            [ myElement
            , myElement
            , el [ alignRight ] <|
                el
                    [ Background.color (rgb255 240 0 245)
                    , Font.color (rgb255 255 255 255)
                    , Border.rounded 3
                    , padding 30
                    ] <|
                text "stylish!"
            ]      
```

This is a summary of the talk [Building a Toolkit for Design](https://www.youtube.com/watch?v=NYb2GDWMIm0) by Matthew Griffith.

## Basics

| Function | Description |
| :--- | :--- |
|  [`layout`](https://package.elm-lang.org/packages/mdgriffith/elm-ui/latest/Element#layout) `: List (Attribute msg) -> Element msg -> Html msg` | Converts Elm-UI into Html. |
|  [`text`](https://package.elm-lang.org/packages/mdgriffith/elm-ui/latest/Element#text) `: String -> Element msg` | Some text. |
|  [`el`](https://package.elm-lang.org/packages/mdgriffith/elm-ui/latest/Element#el) `: List (Attribute msg) -> Element msg -> Element msg` | A basic element similar to `div` from HTML. |
|  [`row`](https://package.elm-lang.org/packages/mdgriffith/elm-ui/latest/Element#row) `: List (Attribute msg) -> List (Element msg) -> Element msg` | A row of elements. |
|  [`column`](https://package.elm-lang.org/packages/mdgriffith/elm-ui/latest/Element#column) `: List (Attribute msg) -> List (Element msg) -> Element msg` | A column of elements. |

## Alignment

{% hint style="info" %}
This subject is explained at [3:26 in the Video](https://youtu.be/Ie-gqwSHQr0?t=206).
{% endhint %}

| Function | Description |
| :--- | :--- |
|  [`centerX`](https://package.elm-lang.org/packages/mdgriffith/elm-ui/latest/Element#centerX) `: Attribute msg` | Horizontally centers anything. |
|  [`centerY`](https://package.elm-lang.org/packages/mdgriffith/elm-ui/latest/Element#centerY) `: Attribute msg` | Vertically centers anything. |
|  [`alignLeft`](https://package.elm-lang.org/packages/mdgriffith/elm-ui/latest/Element#alignLeft) `: Attribute msg` | Alignes anything with the left edge of the screen. |
|  [`alignRight`](https://package.elm-lang.org/packages/mdgriffith/elm-ui/latest/Element#alignRight) `: Attribute msg` | Alignes anything with the right edge of the screen. |
|  [`alignTop`](https://package.elm-lang.org/packages/mdgriffith/elm-ui/latest/Element#alignTop) `: Attribute msg` | Alignes anything with the top edge of the screen. |
|  [`alignBottom`](https://package.elm-lang.org/packages/mdgriffith/elm-ui/latest/Element#alignBottom) `: Attribute msg` | Algines anything with the bottom edge of the screen. |

## Padding and Spacing

{% hint style="info" %}
This subject is explained at [7:32 in the Video](https://youtu.be/Ie-gqwSHQr0?t=452).
{% endhint %}

| Function | Description |
| :--- | :--- |
|   [`padding`](https://package.elm-lang.org/packages/mdgriffith/elm-ui/latest/Element#padding) `: Int -> Attribute msg` | Creates space around the parent |
|   [`spacing`](https://package.elm-lang.org/packages/mdgriffith/elm-ui/latest/Element#spacing) `: Int -> Attribute msg` | Creates space between the children |

## Size

{% hint style="info" %}
This subject is explained at [9:15 in the Video](https://youtu.be/Ie-gqwSHQr0?t=555).
{% endhint %}

| Function | Description |
| :--- | :--- |
|   [`width`](https://package.elm-lang.org/packages/mdgriffith/elm-ui/latest/Element#width) `: Length -> Attribute msg` | Specifies the width of an Element |
|   [`height`](https://package.elm-lang.org/packages/mdgriffith/elm-ui/latest/Element#height) `: Length -> Attribute msg` | Specifies the height of an Element |
|   [`fill`](https://package.elm-lang.org/packages/mdgriffith/elm-ui/latest/Element#fill) `: Length` | Fills the entire space |

## Text Layout

{% hint style="info" %}
This subject is explained at [12:47 in the Video](https://youtu.be/Ie-gqwSHQr0?t=767).
{% endhint %}

| Function | Description |
| :--- | :--- |
|   [`paragraph`](https://package.elm-lang.org/packages/mdgriffith/elm-ui/latest/Element#paragraph) `: List (Attribute msg) -> List (Element msg) -> Element msg` | Defines a Paragraph. Attributes like alignment, padding and spacing will effect the lines of the paragraph |

## Further Reading

* 📖**Book:** [An Introduction to Style Elements for Elm](https://mdgriffith.gitbooks.io/style-elements/) by Matthew Griffith
* 📄**Article:** [Elm-ui: Forget CSS and enjoy creating UIs in pure Elm](https://korban.net/posts/elm/2018-11-17-elm-ui-introduction/) by Alex Korban
* 🎥**Video:** [Building a Toolkit for Design](https://www.youtube.com/watch?v=NYb2GDWMIm0) by Matthew Griffith



