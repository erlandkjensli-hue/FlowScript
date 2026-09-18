# Minimal Application

This example demonstrates a small subset of the current FlowScript semantic model.

~~~flowscript
flowscript:0.1
app:EXAMPLE

page:HOME
    heading:"Home"

    list:ITEMS
        item:ITEM
            goto:DETAIL

page:DETAIL
    heading:"Detail"

    field:VALUE

    button:"Save"
        action:SAVE
        goto:HOME

action:SAVE

presentation:DETAIL
    compact:
        pane:DETAIL
    regular:
        pane:DETAIL
    expanded:
        pane:DETAIL
~~~

## What this demonstrates

- `app` defines the application root.
- `page` defines logical destinations.
- indentation expresses structural containment.
- `list` and `item` express repeated content.
- `goto` expresses explicit navigation.
- `action` represents a system operation.
- `presentation` projects the same semantic page into different presentation modes.

The example is intentionally small. It does not imply that this syntax or vocabulary is finalized.
