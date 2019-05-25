# How to turn a Msg into a Cmd Msg?

{% hint style="danger" %}
**Only Use Sparsely:** Better split the update function into multiple smaller functions.
{% endhint %}

{% tabs %}
{% tab title="Problem" %}
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

{% tab title="Solution" %}
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

{% tab title="Alternative Solution" %}
```text
type Msg =
  LoginSucceeded User
  | InfoMessage String
  
update : Msg -> Model -> (Model, Cmd Msg)
update msg model =
  case msg of
    LoginSucceeded newUser ->
      ( model
        |> updateUser newUser
        |> updateMessage "Login Successful"
      , Cmd.none
      )
    InfoMessage message ->
      ( model |> updateMessage message
      , Cmd.none
      )

updateUser : User -> Model -> Model
updateUser user model =
  { model | currentUser = user }

updateMessage : String -> Model -> Model
updateMessage message model =
  { model | message = Just message }
```

{% hint style="success" %}
**This is the a better solution**
{% endhint %}
{% endtab %}
{% endtabs %}

## Question

How can I display a Message after a user logged in?

## Answer

Use the following function:

```text
sendMsg : msg -> Cmd msg
sendMsg msg =
  Task.succeed msg
  |> Task.perform identity
```

## Further reading

* [How to turn a Msg into a Cmd Msg in Elm?](https://medium.com/elm-shorts/how-to-turn-a-msg-into-a-cmd-msg-in-elm-5dd095175d84) by Wouter In t Velt

