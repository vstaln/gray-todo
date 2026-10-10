<div align="center">
  <img alt="gray-todo" src="assets/icon.svg" width="120" height="120" />
  <h1>gray-todo</h1>
  <p><strong>A per-project todo list for the agent and the user.</strong></p>
  <p>
    <a href="https://gray.alignment.id">Website</a> ·
    <a href="https://gray.alignment.id/plugins/gray-todo">Store</a> ·
    <a href="https://github.com/vstaln/gray-todo">Source</a> ·
    <a href="https://github.com/vstaln/gray">gray</a>
  </p>
  <p>
    <a href="LICENSE"><img alt="License: MIT" src="https://img.shields.io/badge/license-MIT-1c1c20?style=flat-square&labelColor=0a0a0b" /></a>
    <a href="https://www.rust-lang.org"><img alt="Built with Rust" src="https://img.shields.io/badge/built%20with-rust-1c1c20?style=flat-square&labelColor=0a0a0b&logo=rust&logoColor=d4a373" /></a>
    <a href="https://gray.alignment.id/plugins/gray-todo"><img alt="gray plugin" src="https://img.shields.io/badge/gray-plugin-1c1c20?style=flat-square&labelColor=0a0a0b&color=7aa2f7" /></a>
  </p>
</div>

<br/>

```bash
gray plugin install gray-todo
```

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

## Tags

`gray` `plugin` `todo` `rust`

---
Part of the [gray](https://github.com/vstaln/gray) plugin ecosystem —
the open-source AI agent harness. <https://gray.alignment.id>
