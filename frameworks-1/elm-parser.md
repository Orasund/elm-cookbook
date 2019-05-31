# elm/parser

> Regular expressions are quite confusing and difficult to use. This library provides a coherent alternative that handles more cases and produces clearer code.
>
> \(Readme.md from the [package](https://package.elm-lang.org/packages/elm/parser/latest/)\)

```text
type alias Point =
  { x : Float
  , y : Float
  }

point : Parser Point
point =
  succeed Point
    |. symbol "("
    |. spaces
    |= float
    |. spaces
    |. symbol ","
    |. spaces
    |= float
    |. spaces
    |. symbol ")"
```

This is a summary of the talk [Demystifying Parsers](https://www.youtube.com/watch?v=M9ulswr1z0E) by Tereza Sokol.

## Basics

<table>
  <thead>
    <tr>
      <th style="text-align:left">Function</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"> <a href="https://package.elm-lang.org/packages/elm/parser/latest/Parser#run">run</a> :
        Parser a -&gt; String -&gt; Result (List DeadEnd) a</td>
      <td style="text-align:left">Runs the Parser on a String</td>
    </tr>
    <tr>
      <td style="text-align:left"> <a href="https://package.elm-lang.org/packages/elm/parser/latest/Parser#int">int</a> :
        Parser Int</td>
      <td style="text-align:left">Parser for Int</td>
    </tr>
    <tr>
      <td style="text-align:left"> <a href="https://package.elm-lang.org/packages/elm/parser/latest/Parser#float">float</a> :
        Parser Float</td>
      <td style="text-align:left">Parser for Float</td>
    </tr>
    <tr>
      <td style="text-align:left"> <a href="https://package.elm-lang.org/packages/elm/parser/latest/Parser#oneOf">oneOf</a> :
        List (Parser a) -&gt; Parser a</td>
      <td style="text-align:left">
        <p>Trys different Parsers. Uses the first that succeeds.</p>
        <p>Once the parser matches the first element, there is no going back!</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"> <a href="https://package.elm-lang.org/packages/elm/parser/latest/Parser#andThen">andThen</a> :
        (a -&gt; Parser b) -&gt; Parser a -&gt; Parser b</td>
      <td style="text-align:left">Applys the first parser and if successful applies the second. Fails if
        one of the parsers fails.</td>
    </tr>
  </tbody>
</table>## Pipeline

{% hint style="info" %}
This subject is explained at [4:48 in the Video](https://youtu.be/M9ulswr1z0E?t=288).
{% endhint %}

<table>
  <thead>
    <tr>
      <th style="text-align:left">Function</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"> <a href="https://package.elm-lang.org/packages/elm/parser/latest/Parser#succeed">succeed</a> :
        a -&gt; Parser a</td>
      <td style="text-align:left">
        <p>Starts the Pipeline. <code>a -&gt; Parser a</code> is the constructor.</p>
        <p>For a record <code>Point</code> use the structur called <code>Point .</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"> <a href="https://package.elm-lang.org/packages/elm/parser/latest/Parser#(|=)">(|=)</a> :
        Parser (a -&gt; b) -&gt; Parser a -&gt; Parser b</td>
      <td style="text-align:left">Keeps an element.</td>
    </tr>
    <tr>
      <td style="text-align:left"> <a href="https://package.elm-lang.org/packages/elm/parser/latest/Parser#(|.)">(|.)</a> :
        Parser keep -&gt; Parser ignore -&gt; Parser keep</td>
      <td style="text-align:left">Eats a character and throws it away.</td>
    </tr>
    <tr>
      <td style="text-align:left"> <a href="https://package.elm-lang.org/packages/elm/parser/latest/Parser#spaces">spaces</a> :
        Parser ()</td>
      <td style="text-align:left">Represents whitespace</td>
    </tr>
  </tbody>
</table>## Strings

{% hint style="info" %}
This subject is explained at [7:20 in the Video](https://youtu.be/M9ulswr1z0E?t=440).
{% endhint %}

| Function | Description |
| :--- | :--- |
|  [chompWhile](https://package.elm-lang.org/packages/elm/parser/latest/Parser#chompWhile) : \(Char -&gt; Bool\) -&gt; Parser \(\) | Looks for zero or more characters that succeeds the check. Stops as soon as the check fails. |
|  [chompIf](https://package.elm-lang.org/packages/elm/parser/latest/Parser#chompIf) : \(Char -&gt; Bool\) -&gt; Parser \(\) | Reads one symbols. |
|  [getChompedString](https://package.elm-lang.org/packages/elm/parser/latest/Parser#getChompedString) : Parser a -&gt; Parser String | Returnes the string that got chomped |

## Loops

{% hint style="info" %}
This subject is explained at [14:10 in the Video](https://youtu.be/M9ulswr1z0E?t=440).
{% endhint %}

| Function | Description |
| :--- | :--- |
|  [loop](https://package.elm-lang.org/packages/elm/parser/latest/Parser#loop) : state -&gt; \(state -&gt; Parser \([Step](https://package.elm-lang.org/packages/elm/parser/latest/Parser#Step) state a\)\) -&gt; Parser a | Takes an initial state and a parser step and returns a parser.  |
| type [Step](https://package.elm-lang.org/packages/elm/parser/latest/Parser#Step) state a = Loop state \| Done a | A parser step just specifies if it should continue looping or stop. |

## Error messages

{% hint style="info" %}
This subject is explained at [17:04 in the Video](https://youtu.be/M9ulswr1z0E?t=1024).
{% endhint %}

| Function | Description |
| :--- | :--- |
|  [problem](https://package.elm-lang.org/packages/elm/parser/latest/Parser#problem) : String -&gt; Parser a | Returns the error message and ends the parsing. The argument is the actuall message. |

## Further Reading

* **Video:** [Demystifying Parsers](https://www.youtube.com/watch?v=M9ulswr1z0E) by Tereza Sokol

