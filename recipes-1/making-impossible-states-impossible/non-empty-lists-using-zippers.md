# Non empty lists using Zippers

{% tabs %}
{% tab title="Problem" %}
```text
type alias Book =
    List Page

currentPage : Book -> Page
currentPage book =
    case book of
        page :: _ ->
            page
        [] ->
            Debug.todo "this is a dead end."
   
```
{% endtab %}

{% tab title="Solution" %}
```text
type alias Book =
    { previous : List Page
    , current : Page
    , next : List Page
    }

currentPage : Book -> Page
currentPage book =
    book.current
```
{% endtab %}
{% endtabs %}

## Question

How can I  ensure that a list always contains at least one element.

## Answer

Such a list is called a **Zipper List**:

```text
type alias ZipperList a =
    { previous : List a
    , current : a
    , next : List a
    }
```

{% hint style="info" %}
**Package:** Use [miyamoen/select-list](https://package.elm-lang.org/packages/miyamoen/select-list/latest) for a complete implementation of a Zipper List.
{% endhint %}

## Further reading

* **Package:** [miyamoen/select-list](https://package.elm-lang.org/packages/miyamoen/select-list/latest)
* **Package:** [wernerdegroot/listzipper](https://package.elm-lang.org/packages/wernerdegroot/listzipper/latest/)

