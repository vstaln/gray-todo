<p align="center">
  <img src="assets/gray-logo.svg" alt="gray" width="96">
</p>
<h1 align="center">gray-todo</h1>
<p align="center">A per-project todo list — `todo` tool plus an interactive `/todos` picker.</p>
<p align="center">
  <a href="https://github.com/vstaln/gray-todo/blob/main/LICENSE"><img alt="MIT License" src="https://img.shields.io/badge/license-MIT-blue.svg"></a>
  <img alt="gray plugin" src="https://img.shields.io/badge/gray-plugin-7aa2f7.svg">
  <img alt="rust" src="https://img.shields.io/badge/built%20with-rust-orange.svg">
</p>

A per-project todo list for the agent and the user — a `todo` tool plus a
`/todos` command.

## Tools

`todo` — `{action: "list"|"add"|"toggle"|"clear", text?, id?}` with
semantics: ids auto-increment, `toggle` flips `done`, `clear` empties the list
and resets the counter to 1.

## Commands

`/todos` — with the `host.ask` capability granted it opens a picker
("Toggle which todo?") whose options are every todo (`#<id> <text>`, marked
done/pending) plus "just show". Picking a todo toggles it through the same
code path as `toggle` and re-renders the list; picking "just show", or any
ask failure (ungranted capability, timeout, empty answer), falls back to the
plain render: a `done/total completed` line plus one `✓`/`○` line per todo.

## State

State persists per project directory at
`~/.gray/todos/<sha1-of-cwd>.json` (honors `$GRAY_HOME`), written after every
mutation. The list is stable across sessions in the same cwd.

## Wire methods

`plugin/manifest`, `tool/call`, `command/run`, `plugin/shutdown`, plus
sidecar→host `host/ask` for the `/todos` picker. Protocol 1.1. Capability:
`host.ask` (the command degrades to the text render without it).

## Install

```sh
gray plugin install todo
gray plugin capabilities todo --all   # grants host.ask → interactive /todos
```

## Develop

```sh
cargo test
gray account check      # entry point + manifest handshake
gray account publish    # check → build → release → publish to the gray registry
```

---
Part of the [gray](https://github.com/vstaln/gray) plugin ecosystem —
the open-source AI agent harness. <https://gray.alignment.id>
