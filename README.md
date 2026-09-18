# FlowScript

**A human-readable semantic language for describing applications before implementation.**

FlowScript is an experimental notation for describing an application's semantic model: its structure, navigation, behavior, state, context, interactions, and presentation.

The goal is to provide a compact, readable representation that can be understood by people and interpreted deterministically by tools.

> **Status: Experimental / Working Draft**
>
> FlowScript 0.1 is a design experiment, not a finalized language or standard.

## Why FlowScript?

Traditional flowcharts are primarily concerned with sequence and decision paths. FlowScript explores a broader model that can describe:

- application hierarchy
- logical pages and destinations
- UI structure
- navigation
- actions and events
- conditions and state
- context and data references
- validation
- lists and dynamic behavior
- errors and feedback
- responsive presentation

The central idea is:

> **Describe what an application is and how it behaves before deciding how it is implemented.**

FlowScript is intended to remain independent of frameworks, programming languages, databases, and visual design tools.

## Design principles

FlowScript currently follows these principles:

1. **Semantics before syntax.**
2. **Simple written form, strict meaning.**
3. **Explicit navigation.**
4. **Structure is not navigation.**
5. **State is not the same as condition.**
6. **Hierarchy is not the same as presentation.**
7. **Implementation details stay outside the semantic model.**
8. **Identifiers remain stable and independent from labels.**
9. **Human readability is a primary requirement.**
10. **Unresolved ideas remain explicitly provisional.**

## Current specification

The current working specification is:

[`docs/FLOWSCRIPT-0.1-DRAFT-SPECIFICATION-2026-09-18.md`](docs/FLOWSCRIPT-0.1-DRAFT-SPECIFICATION-2026-09-18.md)

It defines the current semantic model, proposed vocabulary, syntax direction, responsive presentation model, validation concepts, and future extensions.

## What FlowScript is not

FlowScript is not intended to replace:

- TypeScript or other programming languages
- HTML or CSS
- backend code
- API or database schemas
- pixel-perfect design tools

It is a semantic layer above implementation.

## Project direction

The immediate goal is not to build a parser first.

The current development order is:

**concepts → semantic model → vocabulary → syntax → grammar → validation → tooling**

The next major step is to test whether a small semantic core can describe multiple application types without becoming domain-specific or turning into a programming language.

## Roadmap

See [`ROADMAP.md`](ROADMAP.md) for the current development direction.

## Community

Use GitHub Discussions for open-ended questions, semantic design, terminology, and broader project discussion. Use Issues for concrete, actionable problems and tasks.

## Contributing

FlowScript is intended to be developed openly.

Discussion, criticism, examples, alternative formulations, semantic proposals, and implementation experiments are welcome.

Please see [`CONTRIBUTING.md`](CONTRIBUTING.md) before opening an issue or pull request.

## Author

**Erland Kjensli**  
Project initiator and original author  
GitHub: [@erlandkjensli-hue](https://github.com/erlandkjensli-hue)

## License

Unless otherwise stated, software in this repository is licensed under the Apache License 2.0.

Documentation and specification licensing is documented separately in the repository.

See [`LICENSE`](LICENSE) and [`LICENSE-DOCUMENTATION.md`](LICENSE-DOCUMENTATION.md).
