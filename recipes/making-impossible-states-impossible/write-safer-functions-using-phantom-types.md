# Write safer functions using Phantom Types

{% tabs %}
{% tab title="Problem" %}
```text
type alias LoginForm =
    { username : String
    , password : String
    , isValid : Bool
    }

{-| Creates a User

Only use this function if the LoginForm was validated.
-}
createUser : LoginForm -> User
createUser loginForm =
    if loginForm.isValid then
        User.create loginForm.username
    else
        Debug.todo "This should not happen."
```
{% endtab %}

{% tab title="Solution" %}
```text
type LoginForm valid =
    LoginForm
        { username : String
        , password : String
        }

type Valid = Valid

{-| Creates a User
-}
createUser : LoginForm Valid -> User
createUser (LoginForm loginForm) =
        User.create loginForm.username
        
validate : LoginForm () -> LoginForm Valid
validate =
    ...
```
{% endtab %}
{% endtabs %}

## Question

How can I  ensure that a user can only be created with a valid login form?

## Answer

Use a so called Phantom Type:

```text
type LoginForm valid = --valid is not used in the definition
    LoginForm
        { username : String
        , password : String
        }
        
type Valid = Valid
```

Use `LoginForm ()` for unvalidated forms and `LoginForm Valid` for validated ones.







