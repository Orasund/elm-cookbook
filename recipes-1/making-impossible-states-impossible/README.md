# Making impossible states Impossible

A good coding practice in Elm is to define the Model such that impossible states can not occur.

Instead of writing

```text
type alias ServerRequest =
    { Response (Maybe String)
    , Failed (Maybe Error)
    }
    -- There can not be a Response and an Error at the same time.
```

one can use

```text
type ServerRequest =
    Waiting
    Response String
    Failed Error
```

In this section we will look at common ways how to avoid impossible states.

## Further reading

* **Video:** [Making impossible states impossible](https://www.youtube.com/watch?v=IcgmSRJHu_8) by Richard Feldman
* **Example:** [Tic Tac Toe](https://discourse.elm-lang.org/t/tictactoe-should-the-winner-be-part-of-the-model/3519/6)
* **Example:** [Subset of {a,b}](https://www.reddit.com/r/elm/comments/bhpc7s/help_designing_my_model/)



