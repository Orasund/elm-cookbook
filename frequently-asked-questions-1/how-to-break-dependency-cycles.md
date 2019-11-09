# How to break Dependency Cycles?

{% tabs %}
{% tab title="Problem" %}
```text
import User exposing (User)

type Model =
    Maybe User

type Msg =
    UserSpecific User.Msg
    Login String String

update : Msg -> Model -> (Model,Cmd Msg)
update msg =
    case msg of
        UserSpecific userMsg ->
            User.update userMsg
        Login name pass ->
            Debug.todo "login user"
```

```
import Main exposing (Msg)

type User =
    ..

type Msg =
    ..
    
updateUser : Msg -> User -> (Model,Cmd Msg)

viewUser : User -> Html Msg
```
{% endtab %}

{% tab title="Solution" %}
```text
import User exposing (User)

type Model =
    Maybe User

type Msg =
    UserSpecific User.Msg
    Login String String

update : Msg -> Model -> (Model,Cmd Msg)
update msg =
    case msg of
        UserSpecific userMsg ->
            User.update Just UserSpecific userMsg 
        Login name pass ->
            Debug.todo "login user"
```

```
type User =
    ..

type Msg =
    ..
    
updateUser : (User -> model) -> (Msg -> msg) -> Msg -> User -> (model,Cmd msg)
```
{% endtab %}

{% tab title="Alternative Solution" %}
```text
import User exposing (User)

type Model =
    Maybe User

type Msg =
    UserSpecific User.Msg
    Login String String

update : Msg -> Model -> (Model,Cmd Msg)
update msg =
    case msg of
        UserSpecific userMsg ->
            let
                (user,msg) = User.update userMsg
            in
            (Just User,UserSpecific msg)
        Login name pass ->
            Debug.todo "login user"
```

```
type User =
    ..

type Msg =
    ..
    
updateUser : Msg -> User -> (User,Cmd User.Msg)
```
{% endtab %}
{% endtabs %}

## Question

How to break out of the dependency cycles?

## Answer

1. Isolate the elements that both of the source modules need access to.
2. Move the shared elements into a new module.
3. Make the source module depend on the new Module.

## Further Reading

* **Article:** [High-Level Dependency Strategies in Elm](https://medium.com/@matthew.buscemi/high-level-dependency-strategies-in-elm-1135ec877d49) by Matthew Buscemi

