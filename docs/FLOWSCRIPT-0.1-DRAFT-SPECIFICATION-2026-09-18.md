# FlowScript 0.1 — Draft Specification and Technical Documentation

**Author:** Erland Kjensli  
**Role:** Project initiator and original author  
**Repository:** https://github.com/erlandkjensli-hue/FlowScript  

**Status:** EXPERIMENTAL / WORKING DRAFT  
**Date:** 2026-09-18  
**Document type:** Semantic model + syntax guide + technical specification  
**Working name:** FlowScript

> This is the first serious specification draft for the FlowScript idea. It is deliberately broad and exploratory. Syntax, vocabulary and semantics are proposals unless explicitly described as established from the source material.

---

## 1. Executive summary

FlowScript is proposed as a human-readable, structurally hierarchical notation for describing applications.

It is intentionally broader than a conventional flowchart.

A traditional flowchart mainly answers: “Where does the user go next?”

FlowScript is intended to describe:

- application hierarchy
- pages and destinations
- UI structure
- navigation
- user actions
- conditions
- state
- context and dataflow
- events and listeners
- validation
- dynamic lists
- feedback and errors
- modal/dialog behavior
- interaction behavior
- responsive presentation
- global and contextual navigation

The central proposition is:

**FlowScript describes what an application is, how it behaves, how users move through it, and how the same logical model can be presented in different environments.**

FlowScript should be tested against real-world applications as early as possible.

---

## 2. Why this is more than a flowchart

The original notation already mixed structural and behavioral information.

It used concepts corresponding to:

- page hierarchy
- global areas
- fields
- buttons
- icons
- lists
- navigation
- actions
- conditions
- remembered context
- sorting
- listeners
- design notes

The corrected version made these more formal with concepts such as condition, state, back, goto, action, section, area, list, component and validation.

The important discovery is that these are not merely graphical flowchart concerns.

The notation is starting to describe an **application semantic model**.

That distinction is foundational.

---

## 3. Source history

The concept originated in two source documents:

1. The original informal notation — the initial human-created notation.
2. The first corrected/systematized version — an early formalization of the original notation.

An early systematized version used a different working name. The name FlowScript is used here as the current working name.

The original material should remain valuable because its informality exposed concepts that a more formal rewrite might accidentally remove.

This specification therefore tries to preserve the useful ideas from both generations.

---

## 4. Primary goals

FlowScript SHOULD make it possible to describe:

1. application hierarchy
2. navigation
3. UI structure
4. behavior
5. conditions
6. state
7. context/dataflow
8. events/interactions
9. responsive presentation
10. reusable global structures

It SHOULD be readable by:

- product people
- UX designers
- developers
- technical writers
- future tooling

It SHOULD be useful in Git because it is:

- textual
- diffable
- reviewable
- versionable
- easy to edit

---

## 5. Non-goals

FlowScript is not intended to replace:

- TypeScript
- React
- CSS
- HTML
- backend code
- API schemas
- database schemas
- pixel-perfect visual design tools

FlowScript may describe semantics that are later implemented by those technologies, but it should remain above implementation details.

A FlowScript document should ideally remain meaningful even if the implementation stack changes.

---

## 6. Semantics versus syntax

This is a central distinction.

### Syntax

Syntax is how FlowScript is written.

Examples:

    page:DETAIL
    goto:DETAIL
    action:SUBMIT_VALUE
    condition:USER_LOGGED_IN

Syntax answers:

**What is a valid FlowScript document?**

### Semantics

Semantics is what those constructs mean.

For example:

    goto:DETAIL

means:

**Transition the current navigation context to the logical destination identified as DETAIL.**

And:

    state:DISABLED

means:

**The referenced object is currently present but not actionable.**

So:

**syntax = form**

**semantics = meaning**

A robust FlowScript specification must define semantics before it freezes grammar details.

---

## 7. Recommended design order

The language SHOULD be developed in this order:

    concepts
      ↓
    semantic model
      ↓
    vocabulary
      ↓
    syntax
      ↓
    grammar
      ↓
    validation
      ↓
    tooling

This order matters.

If syntax is frozen too early, the language can become elegant-looking while still being semantically incomplete.

---

## 8. The semantic layers

The proposed model contains several distinct dimensions.

### 8.1 Structure

What exists and where it belongs.

### 8.2 Navigation

Where the user can move.

### 8.3 Behavior

What actions and events do.

### 8.4 Logic

Conditions and states.

### 8.5 Context

Data and references carried between nodes.

### 8.6 Interaction

Gestures, selection, opening, dragging and similar user interaction.

### 8.7 Presentation

How the semantic model is projected onto a particular device or viewport.

These dimensions are related, but MUST NOT be silently collapsed into one concept.

---

## 9. A crucial architectural distinction: hierarchy != presentation

This is one of the most important discoveries from the application-design work that led to FlowScript.

The logical hierarchy may be:

    HOME
      LIST
        DETAIL
      CONTACTS
      SETTINGS

The presentation may be:

### Compact

    DETAIL

### Regular

    ITEM_LIST | DETAIL

### Expanded

    MAIN_MENU | ITEM_LIST | DETAIL

The hierarchy did not change.

Only the number of simultaneously visible levels changed.

Therefore a future FlowScript implementation SHOULD treat:

- hierarchy
- availability/state
- presentation

as separate semantic dimensions.

---

## 10. Structural tree versus navigation graph versus state graph

A mature FlowScript document is probably not one graph.

It is at least three structures:

### A. Structural tree

What contains what.

Example:

    page:LIST
      list:LIST
        item:DETAIL

### B. Navigation graph

What can lead to what.

Example:

    LIST -> DETAIL
    DETAIL -> LIST

### C. State graph

How state changes.

Example:

    DETAIL_ACTIVE -> ITEM_ARCHIVED

These three structures may overlap, but they are not identical.

This is a major semantic rule.

---

## 11. Application root

A future canonical document SHOULD be able to start with application metadata.

Candidate:

    flowscript:0.1
    app:EXAMPLE_APP

Metadata MAY later include:

- version
- author
- language
- minimum supported parser version
- document status

The exact metadata syntax is not final.

---

## 12. Identifiers

Every major semantic object SHOULD have an identifier.

Examples:

- DETAIL
- HOME
- USER_LOGGED_IN
- SUBMIT_VALUE
- CURRENT_USER

Identifiers SHOULD be:

- stable
- unique within their namespace
- implementation-independent
- safe for references

Display labels SHOULD be separate.

Example:

    page:DETAIL
        label:"Detail"

This lets Norwegian labels change without breaking references.

---

## 13. Labels and localization

UI labels are presentation/content.

Identifiers are semantic identity.

Therefore:

    page:DETAIL
        label:"Detail"

is preferable to using the label itself as the identity.

Future localization MAY use:

    label-key:DETAIL_TITLE

instead of hard-coded text.

No final localization syntax is specified in 0.1.

---

## 14. Basic syntax philosophy

The original notation's simplicity is worth keeping.

The recommended style remains:

    keyword:value

with indentation expressing nesting.

Example:

    page:DETAIL
        heading:"[USER_NAME]"
        icon:BACK
            back:LIST

The syntax SHOULD be:

- easy to scan
- cheap to type
- easy to diff
- visually hierarchical
- possible to parse deterministically

Avoid unnecessary punctuation.

---

## 15. Indentation semantics

Indentation SHOULD be semantic.

A child belongs to the closest enclosing structural node.

For example:

    page:DETAIL
        list:ITEMS
            item:ITEM

means:

- DETAIL contains ITEMS
- ITEMS contains repeated ITEM items

Indentation SHOULD NOT be used merely for visual formatting.

A future parser must be able to determine nesting unambiguously.

---

## 16. Candidate lexical rules

This is provisional.

### Keywords

Lowercase ASCII identifiers.

Examples:

    page
    section
    action
    condition

### IDs

Uppercase semantic identifiers.

Examples:

    DETAIL
    CURRENT_ITEM
    USER_LOGGED_IN

### Labels

Quoted human-readable strings.

Example:

    "Start"

### Comments

Candidate:

    # this is a note

A future grammar may support block comments.

---

## 17. Core structural vocabulary

The current candidate vocabulary is:

    app
    global
    page
    section
    area
    list
    item
    component

### app

The root application.

### global

Application-wide reusable structure.

### page

A logical navigable destination.

### section

A major semantic grouping inside a page.

### area

A meaningful UI region.

### list

A repeating collection.

### item

An element of a list.

### component

A reusable semantic UI unit.

The distinction between area and component needs further refinement in later versions.

---

## 18. Global structures

The original notation contained a global header, main content and footer.

A future document could use:

    global:HEADER
    global:MAIN_CONTENT
    global:FOOTER
    global:COMMON_INTERACTIONS

Global structures SHOULD define reusable shell semantics, not page-specific content.

This separation is valuable because it prevents page definitions from becoming polluted by repeated shell declarations.

---

## 19. UI vocabulary

Candidate primitives:

    heading
    text
    field
    button
    link
    icon
    image
    avatar
    tab
    control
    dropdown
    checkbox
    popup
    modal
    dialog
    alert

The language should distinguish:

- semantic UI meaning
- exact visual component implementation

For example, a button is semantically a button whether React renders it using one component or another.

---

## 20. Page semantics

A page represents a logical destination.

Example:

    page:HOME

A page can contain:

- structural sections
- UI elements
- actions
- navigation
- conditions
- state declarations
- context requirements

A page is not necessarily a physical browser page.

On wide layouts it may become a pane.

This is important.

---

## 21. Pane semantics

A pane is a presentation concept, not necessarily a navigation concept.

For example:

    page:DETAIL

can appear as a full mobile page but as a right-hand pane on desktop.

This suggests a semantic distinction:

    page = logical destination
    pane = visible presentation region

This is one of the strongest candidates for the responsive model.

---

## 22. Navigation vocabulary

Initial candidate:

    goto
    back
    stay

Future candidates:

    replace
    open
    close

### goto

Explicit transition to a logical node.

Example:

    goto:DETAIL

### back

Return to another navigation state.

Example:

    back:LIST

### stay

Remain on the current destination after an action.

Example:

    stay:HOME

---

## 23. Back semantics must be formalized

The word back is ambiguous unless its target semantics are clear.

Possible meanings:

1. previous history entry
2. semantic parent
3. explicit destination

A future version may therefore distinguish:

    back:history
    back:parent
    back:LIST

This is not yet fixed.

---

## 24. Navigation must not be inferred from hierarchy alone

This is a critical rule.

A node being nested inside another node does NOT automatically mean there is a navigation path from the parent to the child.

Example:

    page:SETTINGS
        section:ACCOUNT
            item:PASSWORD

does not itself imply:

    SETTINGS -> PASSWORD

Navigation should be explicit unless the language eventually introduces an explicit shorthand.

This avoids hidden behavior.

---

## 25. Actions

Action means a meaningful operation or side effect.

Examples:

    action:AUTHENTICATE
    action:SUBMIT_VALUE
    action:DELETE_ITEM
    action:CREATE_ITEM

Action semantics SHOULD be independent from the UI control that triggers them.

This makes behavior reusable.

---

## 26. Action versus navigation

These must remain separate.

Example:

    button:"Confirm"
        action:CREATE_ITEM
        goto:DETAIL

This means:

1. an operation is performed
2. the user transitions

A button can perform an action without navigating.

A navigation can also occur without a business action.

---

## 27. Action lifecycle

A future action model SHOULD be able to describe:

- invoked
- pending
- success
- failure
- canceled

Candidate:

    action:CREATE_ITEM
        state:PENDING
        on:SUCCESS
            goto:DETAIL
        on:ERROR
            alert:CREATE_ITEM_FAILED

This may eventually be preferable to overusing condition for asynchronous outcomes.

---

## 28. Conditions

A condition is a logical predicate.

Example:

    condition:USER_LOGGED_IN

Branching:

    condition:USER_LOGGED_IN
        yes:goto:HOME
        no:goto:LOGIN

Conditions answer:

**Is this true?**

They should not be used as generic labels for application state.

---

## 29. State

State describes the current condition of an object or process.

Candidate states:

    LOADING
    EMPTY
    DISABLED
    HIDDEN
    VISIBLE
    SELECTED
    ERROR
    SUCCESS
    ACTIVE
    ARCHIVED

State answers:

**What state is this thing in?**

Condition answers:

**Is this predicate true?**

This distinction is fundamental.

---

## 30. Visibility, availability and state

These concepts may need to be separated in the long term.

For example:

    visibility:HIDDEN
    availability:DISABLED

A control can be:

- visible and enabled
- visible and disabled
- hidden
- available only under a permission condition

Do not force all of these meanings into state.

---

## 31. Access and permissions

A future semantic layer may need:

    access:AUTHENTICATED
    access:PREMIUM
    access:OWNER
    access:MEMBER

Access is not the same as visibility.

A page can be visible but disabled.

A feature can be completely hidden.

An action can be present but forbidden.

These distinctions may become important for mature FlowScript.

---

## 32. Context and dataflow

An earlier notation used a remember-like construct.

A later systematized version used concepts such as prefilled-from-previous-page.

A future FlowScript needs a first-class idea of context.

Examples:

- current user
- current item
- selected item
- selected option
- reference code
- chosen language
- login identifier

Possible conceptual notation:

    context:CURRENT_USER

    field:EMAIL
        value:CONTEXT.LOGIN_EMAIL

The exact syntax is not final.

---

## 33. Typed context

Future proposal:

    context:SELECTED_ITEM
        type:ITEM

    context:CURRENT_ITEM
        type:ITEM

This could support validation.

For example:

    goto:DETAIL(item=SELECTED_ITEM)

A future parser could check that DETAIL expects ITEM.

---

## 34. Parameterized pages

Earlier examples already imply parameterized destinations such as DETAIL[this item].

A cleaner future concept may be:

    page:DETAIL(item:ITEM)

Then:

    goto:DETAIL(item=SELECTED_ITEM)

This is a strong candidate for the final model.

---

## 35. Domain entities

FlowScript may eventually distinguish UI objects from domain objects.

Candidate:

    entity:USER
    entity:ITEM
    entity:DETAIL
    entity:ITEM
    entity:REQUEST

Example:

    list:LIST
        item:DETAIL
            content:USER
            content:LAST_VALUE

This should not become a database schema language.

It simply allows UI semantics to refer to domain concepts.

---

## 36. Events

FlowScript should distinguish user events from actions.

Candidate events:

    on:CLICK
    on:CHANGE
    on:SUBMIT
    on:SELECT
    on:DRAG
    on:LOAD
    on:DATA_UPDATED
    on:AUTH_SUCCESS

Example:

    field:ITEM_SEARCH
        on:CHANGE
            action:SEARCH_ITEMS

This is semantically clearer than embedding implementation-specific listener names.

---

## 37. List semantics

Lists should be modeled as semantic objects.

Candidate properties:
    source
    item
    sort
    filter
    selection
    empty
    loading
    error
    refresh

Example:

    list:LIST
        source:ACTIVE_ITEMS
        sort:LAST_UPDATED_DESCENDING
        item:DETAIL
            goto:DETAIL

The list definition describes behavior and content once instead of duplicating it per row.

---

## 38. Dynamic search

The original notation used listener-while-typing.

The improved semantic direction is:

    field:ITEM_SEARCH
        on:CHANGE
            action:SEARCH_ITEMS
        source:REMOTE_DATA

A future parser may also support:

    debounce:300ms

This is implementation-adjacent but still semantic enough to document behavior.

---

## 39. Validation

Validation SHOULD be expressible separately from field definition.

Example:

    field:EMAIL
        validation:FORMAT
        validation:AVAILABLE

Future structured form:

    validation:EMAIL
        required:true
        format:EMAIL

Validation may produce:

- invalid state
- inline error
- prevented action

---

## 40. Error handling

Error is not just text.

A robust semantic model should express:

    action:SUBMIT_VALUE
        on:ERROR
            state:ERROR
            alert:SUBMISSION_FAILED

This makes error paths part of the normal flow rather than optional documentation.

---

## 41. Feedback primitives

Candidate feedback types:

    alert
    toast
    message
    confirmation
    error
    success

Example:

    action:DELETE_ITEM
        success:toast:"Item deleted"
        error:alert:"Could not delete item"

The exact syntax remains open.

---

## 42. Confirmation behavior

Confirmation should be a semantic interruption.

Example:

    action:DELETE_ITEM
        confirm:CONFIRM_DELETE_ITEM

    modal:CONFIRM_DELETE_ITEM
        button:"Confirm"
            action:CONFIRM
        button:"Cancel"
            action:CANCEL

A future validator could ensure confirmations have both positive and cancellation paths.

---

## 43. Reusable templates

Global structures and repeated behaviors imply a need for reusable definitions.

Future proposal:

    template:HEADER
        ...

    page:DETAIL
        use:HEADER

Likewise:

    behavior:SUBMIT_VALUE
        ...

This should probably be a later extension, not required in the first parser.

---

## 44. Components versus templates

Potential semantic distinction:

**component** = reusable runtime/UI unit

**template** = reusable structural definition

**behavior** = reusable interaction/logic definition

This separation could prevent one overloaded concept from becoming everything.

---

## 45. Interaction semantics

Potential interactions:

- click
- tap
- select
- submit
- drag
- swipe
- long press
- scroll
- keyboard shortcut
- drop

Example:

    component:EXTRA_FUNCTIONS
        interaction:DRAG

Interactions describe what the user can do.

Actions describe what the system does because of it.

---

## 46. Bottom-sheet example

A list-detail interface experiment exposed a useful semantic component.

Conceptually:

    component:EXTRA_FUNCTIONS
        state:OPEN
        state:CLOSED
        interaction:DRAG
        snap:OPEN
        snap:CLOSED

The exact syntax is not final.

The insight is that interactive components may have their own local state machine.

---

## 47. State machines

A mature FlowScript may need explicit state transitions.

Possible future:

    state-machine:DETAIL
        state:ACTIVE
        state:ARCHIVED

        ACTIVE
            on:ARCHIVE
                -> ARCHIVED

        ARCHIVED
            on:UNARCHIVE
                -> ACTIVE

This is deliberately future-facing.

It may prove unnecessary if local state can be expressed more simply.

---

## 48. Temporal behavior

Some application behavior has a time dimension.

Examples:

- splash duration
- delayed actions
- debounce
- timeout
- animation
- countdown

Candidate properties:

    duration:2s
    delay:500ms
    timeout:10s
    debounce:300ms

These should only be standardized if they prove useful in real documents.

---

## 49. Async operations

Actions may be asynchronous.

Potential semantic lifecycle:

    action:AUTHENTICATE
        state:PENDING
        on:SUCCESS
            goto:HOME
        on:ERROR
            alert:LOGIN_FAILED

This is probably a better long-term abstraction than embedding implementation promises/futures.

---

## 50. Persistence

Context may have different lifetimes.

Future proposal:

    persist:TEMPORARY
    persist:SESSION
    persist:USER

Example:

    context:LOGIN_EMAIL
        persist:SESSION

This may eventually replace vague remember-style notation.

---

## 51. Global versus local navigation

Application design work gives a concrete semantic distinction:

- hamburger = global navigation
- back = local navigation
- ellipsis = contextual actions

A future FlowScript model should preserve these categories.

For example:

    global:navigation
        ...

    local:navigation
        ...

    context:DETAIL_ACTIONS
        ...

This is useful both for UX and validation.

---

## 52. Responsive presentation as a projection

Responsive layout should not create duplicate semantic pages.

Example:

    page:LIST
    page:DETAIL

Presentation can then define:

    compact:
        pane:DETAIL

    regular:
        pane:LIST
        pane:DETAIL

    expanded:
        pane:MAIN_MENU
        pane:LIST
        pane:DETAIL

The semantic nodes remain unchanged.

---

## 53. Breakpoint abstraction

Raw pixel widths should not be required by the language.

Instead, FlowScript could use semantic classes:

    compact
    regular
    expanded

or:

    mobile
    tablet
    desktop

The exact vocabulary should be decided later.

Example responsive thresholds are implementation-specific:

- under 800
- 800–1199
- 1200+

They should not be baked into the language core.

---

## 54. Presentation depth

Another useful abstraction may be:

    presentation:
        compact:
            depth:1
        regular:
            depth:2
        expanded:
            depth:3

This says:

**How many hierarchy levels may be simultaneously exposed?**

It is different from explicitly listing panes.

The final model might support both.

---

## 55. Presentation must not redefine hierarchy

Bad conceptual model:

    mobile:PAGE
    desktop:PAGE

Better:

    page:DETAIL

    presentation:
        compact: DETAIL
        expanded: DETAIL as pane

The logical identity remains stable.

---

## 56. Accessibility

A mature FlowScript could optionally describe accessibility semantics:

- accessible name
- role
- focus order
- keyboard interaction
- live updates

Possible future:

    button:"Send"
        accessibility:PRIMARY_ACTION

This should remain semantic and not become a replacement for the accessibility implementation.

---

## 57. Internationalization

IDs must be language-neutral.

Labels can be localized.

Potential future:

    label-key:DETAIL_TITLE

A language file may then provide:

    example-language-a: "Detail"
    en: "Detail"

FlowScript should not need to duplicate pages merely because UI language changes.

---

## 58. Design semantics versus CSS

FlowScript may describe semantic visual roles:

    visual-role:PRIMARY_ACTION
    visual-role:SECONDARY_ACTION
    visual-role:DANGER

It should normally NOT encode raw CSS:

    background:#2f80ed

Design tokens belong elsewhere.

The FlowScript layer describes meaning, not pixel values.

---

## 59. Security boundary

FlowScript may document security intent, but it is not a security enforcement mechanism.

Possible concepts:

    access:AUTHENTICATED
    access:OWNER
    access:PREMIUM

Actual enforcement still belongs to the application/runtime/backend.

---

## 60. Open navigation versus hidden navigation

A page can be reachable even if no current UI element visibly links to it.

This matters for:

- deep links
- notifications
- external links
- restored session state

Future FlowScript may therefore need:

    entry:PUBLIC
    entry:INTERNAL
    entry:DEEPLINK

This is a candidate extension.

---

## 61. Deep links

Possible future semantic:

    entry:DETAIL
        source:NOTIFICATION

    entry:INVITE
        source:DEEPLINK

This would make launch conditions explicit.

---

## 62. Lifecycle

Pages/components may have lifecycle events:

    on:ENTER
    on:EXIT
    on:VISIBLE
    on:HIDDEN

A future model could describe initialization and cleanup without naming React hooks.

---

## 63. History semantics

Navigation history is different from hierarchy.

Example:

    LIST -> DETAIL -> PROFILE -> DETAIL

The parent of PROFILE might not be DETAIL in the structural model even if DETAIL opened it.

Therefore:

- structural parent
- navigation source
- history predecessor

must remain distinguishable.

This is likely to be essential in a mature specification.

---

## 64. Graph cycles are valid

A navigation model is not a tree.

Cycles are normal:

    LIST -> DETAIL
    DETAIL -> LIST

Therefore FlowScript should permit cyclic navigation graphs.

The structural tree remains hierarchical, but transitions form a directed graph.

---

## 65. Machine-readable normalization

A future parser should ideally normalize every declaration into an internal representation such as:

    Node
      id
      type
      label
      parent
      children
      properties
      states
      conditions
      actions
      transitions
      references

The exact AST is not specified in v0.1.

The important idea is that textual syntax and semantic model are separate layers.

---

## 66. Suggested semantic AST

Conceptually:

    App
      Globals[]
      Pages[]
      Components[]
      Entities[]
      Actions[]
      Conditions[]
      States[]
      Presentations[]
      References[]

Each semantic item may have:

- ID
- type
- properties
- relationships
- constraints

---

## 67. Validation rules

A future validator SHOULD be able to detect at least:

### Structural errors

- duplicate IDs
- invalid nesting
- unknown node types

### Reference errors

- undefined goto target
- undefined action
- undefined condition
- undefined state
- undefined component

### Logic errors

- incomplete condition branches
- impossible state
- state without exit where one is required

### Navigation errors

- unreachable page
- broken back route
- unexpected dead end
- invalid target type

### Presentation errors

- unknown pane
- duplicate pane
- invalid responsive reference

---

## 68. Reachability analysis

One strong future feature is automatic reachability analysis.

Starting from START, a tool could determine:

- reachable pages
- unreachable pages
- orphan nodes
- dead ends
- cycles

This is a concrete advantage over ordinary diagrams.

---

## 69. Navigation coverage

A future validator could detect:

- page with no entry path
- page with no exit path
- missing back path
- missing error path
- missing success path

This could be valuable before implementation begins.

---

## 70. Action contracts

Possible future notation:

    action:CREATE_ITEM
        input:OPPONENT
        input:LANGUAGE
        on:SUCCESS
            goto:DETAIL
        on:ERROR
            alert:CREATE_ITEM_FAILED

This turns an action into a reusable semantic contract.

---

## 71. Page contracts

Possible future:

    page:DETAIL
        input:USER
        output:NONE

    page:PROFILE
        input:USER

This could eventually allow tools to validate context requirements.

---

## 72. Typed navigation

Possible future:

    goto:DETAIL(item=SELECTED_ITEM)

A parser could verify:

- DETAIL expects USER
- SELECTED_USER is USER

This would move FlowScript from descriptive-only toward formally analyzable.

This is attractive, but should not be added until the basic model is proven.

---

## 73. Events and external events

The event system may eventually distinguish:

- user event
- system event
- network event
- timer event

Examples:

    on:CLICK
    on:AUTH_SUCCESS
    on:DATA_UPDATED
    on:TIMER

This would help model asynchronous applications.

---

## 74. Concurrency

Modern applications can have multiple independent processes.

FlowScript may eventually need a way to say:

- these actions happen in parallel
- this update can arrive independently
- this page listens for external updates

This is advanced functionality and should be postponed until practical examples require it.

---

## 75. Requirements traceability

A later extension could connect requirements to semantics.

Example:

    requirement:ITEM_ARCHIVE
        implemented-by:ARCHIVE_ITEM
        affects:ITEM_ARCHIVED
        visible-in:LIST

This is potentially valuable for larger projects, but not required for 0.1.

---

## 76. Multiple generated views

If FlowScript becomes a semantic source, one source document could generate several projections:

    FlowScript
       ├── hierarchy diagram
       ├── navigation graph
       ├── state diagram
       ├── responsive pane map
       ├── documentation
       └── test cases

This could become a major long-term advantage.

---

## 77. Canonical-source possibility

Long-term hypothesis:

**FlowScript could become the semantic source of truth, while diagrams and implementation artifacts are projections.**

This is ambitious.

It should NOT be adopted as a requirement until the language has survived real-world tests.

---

## 78. Testing as a design goal

A FlowScript model should make it possible to derive test cases.

For example:

    LOGIN
      -> HOME
      -> LIST
      -> DETAIL

could imply a navigation test.

Conditions:

    LOGIN_SUCCESS
    LOGIN_FAILURE

could imply branch coverage.

This is potentially one of the strongest practical uses of a formal FlowScript.

---

## 79. Reuse and macros

Future versions may support reusable definitions:

    behavior:SUBMIT_VALUE
    template:HEADER
    component:DETAIL_LIST

This would prevent duplication.

The language must however avoid turning into a generic programming language.

Reuse should stay semantic.

---

## 80. Comments and open decisions

The original notation naturally contained product notes and unresolved questions.

That is valuable.

A mature specification may distinguish:

    NOTE
    TODO
    OPEN
    PROPOSAL
    DEPRECATED

These annotations make the document useful during product development.

Normative statements should remain distinct from brainstorming.

---

## 81. Normative language

Future technical documentation should use precise terms:

- MUST
- MUST NOT
- SHOULD
- SHOULD NOT
- MAY

In this 0.1 draft, these words indicate proposal strength, not a ratified standards body.

---

## 82. Proposed v0.1 semantic core

The smallest coherent core currently appears to be:

### Structure

    app
    global
    page
    section
    area
    list
    item
    component

### UI

    heading
    text
    field
    button
    link
    icon
    image
    control

### Navigation

    goto
    back
    stay

### Behavior

    action
    on
    validation

### Logic

    condition
    state

### Context

    context
    input
    selected

### Presentation

    presentation
    pane
    compact
    regular
    expanded

This is the current proposed semantic kernel.

Everything else should be treated as extension until proven necessary.

---

## 83. Proposed syntax style

An illustrative canonical style:

    flowscript:0.1
    app:EXAMPLE_APP

    global:HEADER
        ...

    page:DETAIL
        heading:"[USER_NAME]"

        icon:BACK
            back:LIST

        list:ITEMS
            order:CHRONOLOGICAL
            item:ITEM

        field:TEXT_VALUE

        button:"Send"
            action:SUBMIT_VALUE

The syntax is designed to preserve speed of writing while remaining deterministic and machine-readable.

---

## 84. Proposed responsive style

Illustrative only:

    presentation:DETAIL

        compact
            pane:DETAIL

        regular
            pane:LIST
            pane:DETAIL

        expanded
            pane:MAIN_MENU
            pane:LIST
            pane:DETAIL

Alternative future abstraction:

    presentation:DETAIL
        compact:
            depth:1
        regular:
            depth:2
        expanded:
            depth:3

The specification should eventually decide whether both mechanisms are necessary.

---
## 85. Potential grammar direction

A future formal grammar could roughly be:

    document       := header* definition*
    definition     := app | global | page | entity | action | state | condition | presentation
    node           := keyword ":" value
    child          := INDENT node
    reference      := identifier | parameterized-reference
    label          := quoted-string

This is intentionally incomplete.

The grammar should not be frozen before semantic review.

---

## 86. Strictness versus readability

FlowScript faces a fundamental design choice:

### Very strict

Advantages:
- easy to parse
- easy to validate
- fewer ambiguities

Disadvantages:
- more cumbersome to write
- loses brainstorming speed

### Very loose

Advantages:
- very easy to write
- highly expressive

Disadvantages:
- difficult to validate
- inconsistent documents
- harder tooling

The likely target is:

**human-first but deterministically parseable**

That means syntax can stay simple while semantics remain strict.

---

## 87. Recommended principle: semantic strictness, syntactic simplicity

A particularly promising design goal is:

> Keep the written form simple, but make the underlying meanings strict.

Example:

    goto:DETAIL

looks extremely simple.

A future parser can still require:

- DETAIL exists
- DETAIL is a valid destination
- required parameters exist
- the current context permits the transition

This is a strong combination.

---

## 88. Avoid implementation leakage

Avoid expressions such as:

    react:useEffect
    css:padding
    api:fetch("/...")
    sql:SELECT ...

Those belong below FlowScript.

Prefer:

    on:ENTER
        action:LOAD_ITEM

FlowScript says what the application does.

Implementation decides how.

---

## 89. Potential extension: platform projection

A later version could distinguish platform-specific presentation:

    platform:IOS
    platform:ANDROID
    platform:WEB

However, platform-specific syntax should be avoided until the semantic model has proven that responsive classes alone are insufficient.

---

## 90. Potential extension: permissions

Potential:

    access:AUTHENTICATED
    access:PREMIUM

A future validator might detect that a public navigation path reaches a protected page without an authentication transition.

This could be useful, but security remains external to FlowScript execution.

---

## 91. Potential extension: analytics

An application specification sometimes needs to define meaningful user events.

Future:

    event:DETAIL_OPENED
    event:PROCESS_STARTED

or:

    action:SUBMIT_VALUE
        emit:VALUE_SUBMITTED

This could create traceability without embedding an analytics provider.

Not part of 0.1 core.

---

## 92. Potential extension: deep links and external entry

Future semantic:

    entry:DEEPLINK
        goto:DETAIL(item=...)

    entry:NOTIFICATION
        goto:DETAIL(item=...)

This makes application startup routing explicit.

---

## 93. Potential extension: restoration

Modern applications restore:

- current page
- scroll position
- selected tab
- unfinished draft
- session

Future:

    restore:SESSION

may be useful.

Again, postpone until examples require it.

---

## 94. Potential extension: undo/rollback semantics

Some actions have rollback behavior.

Example:

    action:DELETE_ITEM
        undo:RESTORE_ITEM

This could be useful for toasts and reversible actions.

Not required for core FlowScript.

---

## 95. Potential extension: background processes

Some application logic continues when the current page is not active.

Future concepts:

    background:ITEM_SYNC
    background:PRESENCE_UPDATE

This would help represent real applications, but belongs to an advanced runtime layer.

---

## 96. Potential extension: notification-driven flow

Example:

    notification:DATA_UPDATED
        goto:DETAIL

This ties external events into navigation.

Again, future extension.

---

## 97. Potential extension: offline behavior

A mature application model may need:

- offline
- reconnecting
- queued action
- retry

Potential state:

    state:OFFLINE

and action semantics such as:

    action:SUBMIT_VALUE
        offline:QUEUE

This is useful for real mobile apps.

---

## 98. Potential extension: accessibility state

Future semantics may include:

- focusable
- focused
- aria-like role concepts
- announcement
- keyboard navigation

This should remain semantic rather than markup-specific.

---

## 99. Potential extension: content/state binding

A component could refer to semantic data:

    text:CURRENT_USER.NAME

This could become a declarative content-binding system.

However, avoid accidentally turning FlowScript into a templating language.

---

## 100. Potential extension: design system roles

Semantic visual roles could be:

    visual-role:PRIMARY
    visual-role:SECONDARY
    visual-role:DANGER
    visual-role:SUCCESS

This allows design system integration without encoding CSS.

---

## 101. Potential extension: generated diagrams

If parsed, FlowScript could generate:

- navigation graph
- hierarchy tree
- state diagram
- responsive presentation map
- component tree

This may be more valuable than manually maintaining diagrams.

---

## 102. Potential extension: generated tests

A parser could derive:

- route tests
- condition tests
- state transition tests
- back-navigation tests
- access tests

This makes the specification executable in the broad sense without becoming application code.

---

## 103. Potential extension: code scaffolding

Eventually:

    FlowScript
       ↓
    project scaffold
       ↓
    React / TypeScript

Generated output could include:

- route skeletons
- component placeholders
- action interfaces
- state types
- navigation maps

This should be a future project, not part of the language core.

---

## 104. A complex application as benchmark

A good first benchmark should be complex enough to contain:

- authentication and access control
- a home or dashboard destination
- list-detail navigation
- search and filtering
- forms and validation
- settings and preferences
- reusable components
- dialogs and confirmations
- asynchronous operations
- loading, empty and error states
- responsive multi-pane presentation

If FlowScript can describe such an application cleanly, it has passed a meaningful complexity test.

---

## 105. Second benchmark classes

After the first benchmark, the language SHOULD be tested against:

1. a communication-oriented application
2. a commerce-oriented application
3. a configuration-heavy application
4. a productivity-oriented application
5. a content-oriented application

The purpose is to detect concepts that are genuinely domain-general rather than artifacts of a single application.

---

## 106. What success looks like

A successful FlowScript document should let a reader answer:

### Structure
- What exists?
- What contains what?

### Navigation
- Where can the user go?
- What does back mean?

### Behavior
- What happens when a control is used?

### Logic
- What conditions exist?
- What states exist?

### Context
- What data is required?
- What data is carried?

### Presentation
- What is visible on compact/regular/expanded layouts?

### Failure
- What happens when an action fails?
- What happens while loading?
- What happens when a list is empty?

If those questions can be answered without reading React code, FlowScript is doing useful work.

---

## 107. Major design risks

### Risk 1: becoming a programming language

If every implementation detail can be encoded, FlowScript loses its purpose.

### Risk 2: becoming a prettier wireframe format

If only UI layout is encoded, behavior is lost.

### Risk 3: one overloaded keyword for everything

This creates ambiguity.

### Risk 4: overformalizing too early

This can kill the speed that made the original notation attractive.

### Risk 5: responsive duplication

Defining separate mobile and desktop pages would defeat the semantic hierarchy model.

### Risk 6: hidden inference

If hierarchy silently implies navigation, documents become ambiguous.

### Risk 7: implementation leakage

React/CSS/API details would make the language stack-dependent.

---

## 108. Proposed principles to guard against those risks

1. Semantic meaning first.
2. Explicit navigation.
3. Separate structure from behavior.
4. Separate state from conditions.
5. Separate hierarchy from presentation.
6. Keep IDs independent from labels.
7. Keep implementation details out.
8. Preserve human readability.
9. Allow future machine parsing.
10. Prefer explicitness over magical inference.
11. Support reuse without becoming a programming language.
12. Keep unresolved ideas explicitly marked as proposals.

---

## 109. Possible standard sections inside a FlowScript document

A mature file could use:

    META
    GLOBAL
    ENTITIES
    ACTIONS
    STATES
    CONDITIONS
    PAGES
    PRESENTATION
    REQUIREMENTS
    NOTES

The exact ordering is not mandatory yet.

The original material's global/header-first arrangement is still a useful influence.

---

## 110. Separation of declarations and usages

One design question is whether actions should be declared globally or locally.

Option A:

    button:"Send"
        action:SUBMIT_VALUE

Option B:

    action:SUBMIT_VALUE
        ...

    button:"Send"
        invoke:SUBMIT_VALUE

Option A is easier to write.

Option B enables stronger contracts.

A future language may support both, where the local form references an implicit declaration.

---

## 111. Semantic shorthand

The language may eventually distinguish:

- shorthand syntax for humans
- expanded AST semantics for tools

Example:

    goto:DETAIL

could normalize internally to:

    transition
        type:navigation
        target:DETAIL

This is a powerful reason to keep syntax compact.

---

## 112. Version 0.1 semantic contract

For the first real prototype, the semantic contract SHOULD be limited to:

### Required

- app
- page
- hierarchical nesting
- explicit navigation
- action
- condition
- state
- basic UI nodes
- basic context
- basic presentation

### Optional

- lists
- validation
- events
- modal
- feedback

### Deferred

- macros
- imports
- typed AST
- concurrency
- background processes
- advanced security
- code generation
- analytics
- generated tests

This keeps the concept testable.

---

## 113. Proposed canonical application sample

Illustrative:

    flowscript:0.1
    app:EXAMPLE_APP

    global:HEADER

    page:HOME
        section:ACTIVE_ITEMS
        section:REQUESTS
        section:FINISHED_ITEMS

    page:LIST
        list:LIST
            item:DETAIL
                goto:DETAIL

    page:DETAIL(item:ITEM)
        icon:BACK
            back:LIST

        list:ITEMS
            order:CHRONOLOGICAL
            item:ITEM

        field:TEXT_VALUE

        button:"Send"
            action:SUBMIT_VALUE

    page:CONTACTS
        list:CONTACTS
            item:USER
                goto:PROFILE

    page:PROFILE(item:ITEM)
        button:"Open"
            action:OPEN_OR_CREATE_ITEM
            goto:DETAIL(item=THIS_ITEM)

    presentation:LIST
        compact:
            depth:1
        regular:
            depth:2
        expanded:
            depth:3

This is a semantic experiment, not final grammar.

---

## 114. Potential future semantic categories

The language can grow carefully if real use reveals a need for:

- security
- persistence
- notifications
- deep links
- lifecycle
- concurrency
- offline behavior
- analytics
- requirements
- accessibility
- internationalization
- design-system roles

These should be added as semantic extensions rather than ad hoc keywords.

---

## 115. Formalization strategy

When we are ready to formalize, the process should be:

### Step 1
Freeze the semantic vocabulary.

### Step 2
Define each primitive in English-language normative prose.

### Step 3
Define legal nesting rules.

### Step 4
Define reference resolution.

### Step 5
Define branching semantics.

### Step 6
Define state semantics.

### Step 7
Define presentation semantics.

### Step 8
Write grammar.

### Step 9
Build parser.

### Step 10
Run conformance examples.

This prevents grammar from driving semantics.

---

## 116. Conformance examples

A future specification SHOULD contain canonical examples for:

- authentication
- conditional navigation
- modal confirmation
- form validation
- list loading/empty/error
- list-detail navigation
- state transitions
- responsive multi-pane presentation
- deep link
- permissions

A parser must accept canonical valid examples and reject intentionally invalid examples.

---

## 117. Negative examples

A mature technical spec should contain invalid examples.

For example:

    goto:UNKNOWN_PAGE

should fail because UNKNOWN_PAGE is undefined.

And:

    button:"Send"
        goto:UNKNOWN

should fail reference validation.

Likewise:

    presentation:DETAIL
        pane:UNKNOWN

should fail if UNKNOWN is not a known semantic node.

---

## 118. Why Git is a good home

FlowScript is a good candidate for Git because:

- the hierarchy is text
- diffs show semantic change
- branches can hold design experiments
- reviews can discuss individual lines
- generated diagrams can be derived later

A dedicated FlowScript repository is therefore a suitable laboratory for the concept.

---

## 119. Relationship to application documentation

This document should sit conceptually above application-specific navigation and implementation documentation.

A navigation prototype can demonstrate one concrete application of the ideas.

FlowScript is the candidate general notation that may eventually describe such navigation models across different applications.

Therefore the relationship is:

    FlowScript
        ↓
    application model
        ↓
    navigation prototype
        ↓
    React implementation

This is conceptual, not yet an automated pipeline.

---

## 120. Recommended next experiments

The next phase should NOT be “write more keywords”.

It should be:

### Experiment A
Rewrite a complete application flow entirely in the proposed semantic core.

### Experiment B
Try to describe a responsive list-detail application and its presentation panes.

### Experiment C
Try a small unrelated application.

### Experiment D
List every place where the syntax feels awkward or ambiguous.

### Experiment E
Revise semantics before revising punctuation.

---

## 121. The central test

The most important test is:

**Can a person who has never seen the React implementation understand the application architecture and behavior from FlowScript alone?**

A stronger test:

**Can two different implementations of the same app be described by the same FlowScript?**

If yes, the notation is truly semantic.

---

## 122. The strongest current hypothesis

The strongest version of the idea is not:

**“Let's invent a nicer flowchart.”**

It is:

**“Let's invent a compact language for describing the semantic model of an application before implementation.”**

A flowchart can be generated from that model.

A navigation tree can be generated.

A state diagram can be generated.

Documentation can be generated.

Tests might be generated.

Eventually code scaffolding might be generated.

That is the potentially large idea worth exploring.

---

## 123. Final 0.1 position

FlowScript 0.1 SHOULD therefore be treated as:

- a semantic design experiment
- a human-readable application model
- a possible future DSL
- a candidate source format for multiple projections

It is NOT yet:

- a programming language
- a formal standard
- a compiler input
- a code-generation language
- a finished grammar

---

## 124. Immediate next task

The next serious task should be a **semantic comparison** between:

1. the original informal notation
2. the first systematized notation
3. this FlowScript 0.1 semantic model

The goal is to decide:

- what should be retained
- what should be removed
- what should be generalized
- what needs a clearer semantic definition
- what should become syntax
- what should remain only documentation

Only after that should FlowScript 0.2 or a formal grammar be drafted.

---

# Appendix A — Provisional keyword inventory

### Application / structure

    flowscript
    app
    global
    page
    section
    area
    list
    item
    component
    template
    entity

### UI

    heading
    text
    field
    button
    link
    icon
    image
    avatar
    tab
    control
    dropdown
    checkbox
    popup
    modal
    dialog
    alert

### Navigation

    goto
    back
    stay
    replace
    open
    close

### Behavior

    action
    on
    listener
    validation
    source
    sort
    filter

### Logic/state

    condition
    state
    visibility
    availability
    access

### Context/data

    context
    input
    output
    value
    selected
    persist

### Presentation

    presentation
    pane
    compact
    regular
    expanded
    layout

All Appendix A vocabulary is provisional unless separately ratified.

---

# Appendix B — Conceptual reference model

    APPLICATION
    │
    ├── GLOBAL STRUCTURE
    │     ├── HEADER
    │     ├── MAIN CONTENT
    │     ├── FOOTER
    │     └── COMMON INTERACTIONS
    │
    ├── DOMAIN
    │     ├── ENTITIES
    │     └── CONTEXT
    │
    ├── STRUCTURE
    │     ├── PAGES
    │     ├── SECTIONS
    │     ├── AREAS
    │     ├── LISTS
    │     └── COMPONENTS
    │
    ├── NAVIGATION GRAPH
    │     ├── GOTO
    │     ├── BACK
    │     └── HISTORY
    │
    ├── BEHAVIOR
    │     ├── ACTIONS
    │     ├── EVENTS
    │     └── VALIDATION
    │
    ├── LOGIC
    │     ├── CONDITIONS
    │     └── STATES
    │
    └── PRESENTATION
          ├── COMPACT
          ├── REGULAR
          └── EXPANDED

---

# Appendix C — Design principles to preserve

1. **Human readable first.**
2. **Semantic meaning before syntax elegance.**
3. **Hierarchy is not navigation.**
4. **Navigation is not action.**
5. **State is not condition.**
6. **Visibility is not permission.**
7. **Logical identity is not presentation.**
8. **Responsive layouts are projections of one semantic model.**
9. **Explicit references are safer than magical inference.**
10. **Keep implementation details out of the core language.**
11. **Keep the notation compact enough to write quickly.**
12. **Use Git as a natural source-control environment.**
13. **Test semantics before building tooling.**
14. **Preserve useful ideas from the original informal notation.**
15. **Do not freeze the grammar prematurely.**

---

# Appendix D — Status of this document

This document is deliberately a first draft.