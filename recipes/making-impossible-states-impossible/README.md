# Making impossible states Impossible

A good coding practice in Elm is to define the Model such that impossible states can not occur.

Instead of writing

```text
Type alias ServerRequest =
    { Response (Maybe String)
    , Failed (Maybe Error)
    }
    -- There can not be a Response and an Error at the same time.
```

one can use

```text
Type ServerRequest =
    Waiting
    Response String
    Failed Error
```

In this section we will look at common ways how to avoid impossible states.

## Further reading

* [Making impossible states impossible](https://www.youtube.com/watch?v=IcgmSRJHu_8) by Richard Feldman

