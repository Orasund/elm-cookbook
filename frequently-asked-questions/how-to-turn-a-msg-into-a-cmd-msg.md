# How to turn a Msg into a Cmd Msg?

## The Problem

{% tabs %}
{% tab title="The Problem" %}
```text
type Msg =
  LoginSucceeded User
  | InfoMessage String
  
update : Msg -> Model -> (Model, Cmd Msg)
update msg model =
  case msg of
    LoginSucceeded newUser ->
      ( { model | currentUser = newUser }
      , Cmd.none
      )
    InfoMessage message ->
      ( { model | message = Just message }
      , Cmd.none
      )
```
{% endtab %}

{% tab title="The Solution" %}
```text
type Msg =
  LoginSucceeded User
  | InfoMessage String
  
update : Msg -> Model -> (Model, Cmd Msg)
update msg model =
  case msg of
    LoginSucceeded newUser ->
      ( { model | currentUser = newUser }
      , sendMsg <| InfoMessage "Login Successful"
      )
    InfoMessage message ->
      ( { model | message = Just message }
      , Cmd.none
      )

sendMsg : msg -> Cmd msg
sendMsg msg =
  Task.succeed msg
  |> Task.perform identity
```
{% endtab %}
{% endtabs %}

> How can I display a Message after a user logged in?

## The Solution

Use the following function:

```text
sendMsg : msg -> Cmd msg
sendMsg msg =
  Task.succeed msg
  |> Task.perform identity
```

