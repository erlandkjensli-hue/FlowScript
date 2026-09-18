# Contributing to FlowScript

FlowScript is an experimental language and semantic model developed in the open.

At this stage, semantic discussion is as important as implementation. Contributions that challenge assumptions, reveal ambiguity, or improve generality are especially valuable.

## What contributions are useful?

Useful contributions include:

- identifying ambiguity in the semantic model
- proposing clearer terminology
- proposing or testing alternative syntax
- creating examples from different application domains
- identifying concepts that are too specific, too broad, or redundant
- building parsers, validators, editors, or visualization tools
- improving documentation
- testing whether the model remains implementation-independent

## Before opening an issue or discussion

Please search existing issues and discussions first.

For specification questions, explain:

1. the semantic problem
2. the proposed interpretation
3. a concrete example
4. why the proposal generalizes beyond one application type

The current 0.1 syntax is intentionally provisional. Do not assume that existing syntax is fixed.

## Pull requests

For changes to the specification or terminology:

- explain the motivation
- keep changes focused
- include examples where useful
- distinguish established behavior from proposals
- avoid introducing implementation-specific concepts into the semantic core

Large semantic changes should normally begin as an issue or discussion before a pull request.

## Development philosophy

The current development order is:

**concepts → semantic model → vocabulary → syntax → grammar → validation → tooling**

Prefer a small number of strong semantic primitives over a large keyword inventory.

## Licensing contributions

Unless a separate written agreement states otherwise:

- software contributions are licensed under the Apache License 2.0
- specification, documentation, and example-text contributions are licensed under CC BY 4.0

By contributing, you confirm that you have the right to submit the material and that you understand the applicable license.
