# gray-todo

Per-project todo list for the agent and the user — a `todo` tool plus a
`/todos` command. Port of pi's `todo` extension.

## Tools

`todo` — `{action: "list"|"add"|"toggle"|"clear", text?, id?}` with pi's exact
semantics: ids auto-increment, `toggle` flips `done`, `clear` empties the list
and resets the counter to 1.

## Commands

`/todos` — renders the list: a `done/total completed` line plus one checkbox
line (`✓`/`○`) per todo.

## State

Pi kept todos in session entries so branching rewound them. Gray sidecars have
no session-entry store, so state persists per project directory at
`~/.gray/todos/<sha1-of-cwd>.json` (honors `$GRAY_HOME`), written after every
mutation. The list is stable across sessions in the same cwd.

## Wire methods

`plugin/manifest`, `tool/call`, `command/run`, `plugin/shutdown`. Protocol 1.1.
No capabilities, no host→sidecar requests.

## Install

```sh
gray plugin install todo
```

## Develop

```sh
cargo test
gray account check      # entry point + manifest handshake
gray account publish    # check → build → release → publish to the gray registry
```
