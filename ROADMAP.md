# FlowScript Roadmap

FlowScript is intentionally being developed from semantics toward syntax and tooling.

The roadmap is a working plan, not a commitment to specific release dates.

## 0.1 — Experimental semantic draft

Current focus:

- establish the semantic model
- identify the smallest coherent semantic core
- distinguish structure, navigation, behavior, logic, context, interaction, and presentation
- test whether the model remains implementation-independent
- document unresolved questions explicitly

Status: **Current**

## 0.2 — Semantic consolidation

Planned areas:

- refine terminology
- clarify semantic boundaries
- examine events and interactions
- examine asynchronous behavior
- refine validation
- refine dialogs, feedback, and lists
- examine state transitions

The primary goal is semantic clarity, not keyword expansion.

## 0.3 — Presentation model

Planned areas:

- responsive presentation
- panes
- compact / regular / expanded presentation
- hierarchy-depth abstraction
- global, local, and contextual navigation

The presentation model must remain a projection of the same semantic application model.

## 0.4 — Formal syntax

Planned areas:

- lexical rules
- indentation semantics
- identifiers
- references
- parameters
- comments
- normative syntax definitions

Grammar should follow semantic decisions rather than drive them.

## 0.5 — Validation

Potential capabilities:

- reference validation
- duplicate detection
- structural validation
- navigation reachability
- state analysis
- presentation validation
- invalid-example conformance tests

## 0.6+ — Tooling

Potential capabilities:

- parser
- normalized semantic representation
- hierarchy diagrams
- navigation graphs
- state diagrams
- presentation maps
- generated documentation
- generated tests

## 1.0 — Only after real-world validation

A 1.0 release should require evidence that:

1. the semantic core works across multiple application types
2. major ambiguities have been resolved or explicitly bounded
3. the syntax can be parsed deterministically
4. validation rules are sufficiently stable
5. the model remains independent of any one implementation technology
6. the project has clear documentation and contribution practices

There is intentionally no fixed 1.0 date.

## How to influence the roadmap

The most useful contributions are concrete semantic problems, examples, counterexamples, alternative interpretations, and evidence from implementations or other application domains.

Large changes should normally be discussed before they become pull requests.
