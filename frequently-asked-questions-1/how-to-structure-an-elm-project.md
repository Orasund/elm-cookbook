# How to structure an Elm project?

{% hint style="info" %}
**Do not overuse:** Files are allowed to get quite big without needing to split them.
{% endhint %}

{% tabs %}
{% tab title="Problem" %}
{% code-tabs %}
{% code-tabs-item title="Readme.md" %}
```
check out todo.elm for the full code.
```
{% endcode-tabs-item %}

{% code-tabs-item title="Todo.elm" %}
```text
type Task =
    Task
        { name : String
        , completed : Bool
        }

createTask : String -> Task

completeTask : Task -> Task

type Form a =
    Valid a
    | Invalid a
    | Partial a

validate : (Form a -> Form ()) -> Form a -> Form a

type alias TaskForm =
    { name : String
    , validated : Bool
    }

validateTaskForm : Form TaskForm -> Form ()

type User =
    User
        { name : String
        , lastOnline : Posix
        }

createUser : String -> Posix -> User

type alias UserForm =
    { name : String
    , password : String
    , validated : Bool
    }

validateUserForm : Form UserForm -> Form ()

type alias TodoPageModel =
    { form :
        Form TaskForm
    , todo : Array Task
    , User : User
    }

type alias LoginPageModel =
    Form UserForm

type Model =
    LoginPage LoginPageModel
    | TodoPage TodoPageModel

type LoginPageMsg =
    NameEntered String
    | PasswordEntered String
    | Submited 
    | GotTime Posix

type TodoPageMsg =
    LoggedOut
    | TaskAdded String
    | TaskCompleted Int
    | TaskDeleted

type Msg =
    LoginSpecific LoginPageMsg
    | TodoSpecific TodoPageMsg

init : () -> Model

updateTodoPage : TodoPageModel -> (TodoPageModel, Cmd TodoPageMsg)

updateLoginPage : LoginPageModel -> (LoginPageModel, Cmd LoginPageMsg)

update : Msg -> (Model, Cmd Msg) -> (Model, Cmd Msg)

viewUserForm : Form UserForm -> Html Msg

viewLoginPage : LoginPageModel -> Html Msg

viewTask : Task -> Html Msg

viewTaskForm : Form TaskForm -> Html Msg

viewTodoPage : TodoPageModel -> Html Msg

view : Model -> Html Msg

subscription : Model -> Sub Msgexpos
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}

{% tab title="Solution" %}
{% code-tabs %}
{% code-tabs-item title="Main.elm" %}
```text
importing Todo.Page.Login as LoginPage
importing Todo.Page.Todo as TodoPage

type Model =
    LoginPage LoginPage.Model
    | TodoPage TodoPage.Model

type Msg =
    LoginSpecific LoginPage.Msg
    | TodoSpecific TodoPage.Msg

init : () -> Model

update : Msg -> (Model, Cmd Msg) -> (Model, Cmd Msg)

view : Model -> Html Msg

subscription : Model -> Sub Msg
```
{% endcode-tabs-item %}

{% code-tabs-item title="Page/Login.elm" %}
```
module Todo.Page.Login exposing (Model,Msg,view,update)

importing Todo.Data.Form as Form exposing (Form)
importing Todo.Data.User as User exposing (User,UserForm)

type alias Model =
    Form UserForm
    
type Msg =
    NameEntered String
    | PasswordEntered String
    | Submited 
    | GotTime Posix

update : Model -> (Model, Cmd Msg)

viewUserForm : Form UserForm -> Html Msg

view : Model -> Html Msg
```
{% endcode-tabs-item %}

{% code-tabs-item title="Page/Todo.elm" %}
```
module Todo.Page.Login exposing (Model,Msg,view,update)

importing Todo.Data.Task as Task exposing (Task,TaskForm)
importing Todo.Data.User as User exposing (User,UserForm)

type alias Model =
    { form :
        Form TaskForm
    , todo : Array Task
    , User : User
    }

type Msg =
    LoggedOut
    | TaskAdded String
    | TaskCompleted Int
    | TaskDeleted

update : Model -> (Model, Cmd Msg)

viewTask : Task -> Html Msg

viewTaskForm : Form TaskForm -> Html Msg

view : Model -> Html Msg
```
{% endcode-tabs-item %}

{% code-tabs-item title="Data/Task.elm" %}
```
module Todo.Data.Task exposing (Task,create,complete,validate)

importing Todo.Data.Form as Form exposing (Form)

type Task =

    Task
        { name : String
        , completed : Bool
        }

create : String -> Task

complete : Task -> Task

type alias TaskForm =
    { name : String
    , validated : Bool
    }

validate : Form TaskForm -> Form ()
```
{% endcode-tabs-item %}

{% code-tabs-item title="Data/Form.elm" %}
```
module Todo.Data.Form exposing (Form(..),validate)

type Form a =
    Valid a
    | Invalid a
    | Partial a

validate : (Form a -> Form ()) -> Form a -> Form a
```
{% endcode-tabs-item %}

{% code-tabs-item title="Data/User.elm" %}
```
module Todo.Data.User exposing (User,UserForm,create,validate)

importing Todo.Data.Form as Form exposing (Form)

type User =
    User
        { name : String
        , lastOnline : Posix
        }

create : String -> Posix -> User

type alias UserForm =
    { name : String
    , password : String
    , validated : Bool
    }

validate : Form UserForm -> Form ()

```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}

## Question

How should I structure my Elm project?

## Answer

Use the following file structure:

* Main.elm - Contains the main code
* Data.elm - Contains utility functions for types \(like constants\)
  * Data/.. - Contains types. Group them logically, for example `User` and `UserForm` into `Data/User.elm`.
* View.elm - Contains utility functions for views \(like view specific constants and very general function.\)
  * View/.. - Contains different Views. Sometimes a type has different views. A login page might have a special view for a wrong login.
* Page.elm - Contains utility functions for pages. For page-transitions its handy to store the different models in here.
  * Page/.. - Contains a Model/View/Update for every page.
* .. - in the same style you can add your project specific folders like a separate folder for validation.

## Further reading

* **Document:** [NoRedInk Style Guide](https://github.com/NoRedInk/elm-style-guide/blob/master/README.md)
* **Article:** Tour of an [Open-Source Elm SPA](https://dev.to/rtfeldman/tour-of-an-open-source-elm-spa) by Richard Feldman
* **Video:** [The life of a file](https://www.youtube.com/watch?v=XpDsk374LDE) by Evan Czaplicki

