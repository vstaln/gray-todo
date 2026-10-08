# gray-todo

Per-project todo list for the agent and the user — a `todo` tool plus a
`/todos` command. Port of pi's `todo` extension.

## Tools

`todo` — `{action: "list"|"add"|"toggle"|"clear", text?, id?}` with pi's exact
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

Pi kept todos in session entries so branching rewound them. Gray sidecars have
no session-entry store, so state persists per project directory at
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
