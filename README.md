# Index — A Task List

A small, dependency-free to-do list app built to practice core JavaScript fundamentals: DOM manipulation, delegated event handling, and client-side persistence with `localStorage`. Everything lives in a single HTML file — no build step, no frameworks, no npm install.

## Features

- **Full CRUD**
  - **Create** — type a task and hit Enter or click Add.
  - **Read** — tasks render from a single in-memory array, filtered by the active tab.
  - **Update** — double-click a task's text (or click the pencil icon) to edit it inline. Press Enter to save, Escape to cancel, or click away to save automatically.
  - **Delete** — click the trash icon to remove a single task, or "Clear completed" to remove every finished task at once.
- **Persistence** — every change is saved to `window.localStorage` as JSON and reloaded automatically the next time the page opens.
- **Filtering** — switch between **All**, **Active**, and **Done** without losing the underlying data; the counts and list update instantly.
- **Delegated events** — the task list uses one click listener on the parent `<ul>` instead of a listener per row, so newly added tasks work immediately with no re-binding.
- **Small touches** — items-left counter, disabled "Clear completed" when nothing's completed, keyboard support while editing, and three example tasks seeded on first run.

## Getting started

1. Download `todo-app.html`.
2. Open it directly in any modern browser (double-click the file, or drag it into a browser tab).
3. Add a few tasks, reload the page, and confirm they're still there — that's `localStorage` doing its job.

No server, build tool, or internet connection is required (the Google Fonts used for the headline and mono text will fall back to system fonts if you're offline).

> **Note on Claude's in-chat preview:** the preview panel runs the app inside a sandboxed iframe, where `localStorage` may not persist between reloads. This is a limitation of the preview environment, not the app — once you download the file and open it as a regular page, persistence works normally.

## How it works

| Concern | Where it lives |
|---|---|
| State | A single `tasks` array of `{ id, text, completed, createdAt }` objects |
| Persistence | `loadTasks()` / `saveTasks()` — read/write `tasks` to `localStorage` under the key `todo.tasks.v1` |
| Rendering | `render()` rebuilds the `<ul>` from scratch on every state change, using `createElement` and a `DocumentFragment` |
| Filtering | `getFilteredTasks()` derives a view from `tasks` + `currentFilter` — the source array is never mutated by filtering |
| Events | One `click` listener on the list reads a `data-action` attribute (`toggle`, `edit`, `delete`) off whatever was clicked, plus a `dblclick` listener for inline editing |

### Data shape

```json
{
  "id": "a1b2c3d4-...",
  "text": "Buy oat milk",
  "completed": false,
  "createdAt": 1737936000000
}
```

## Browser support

Works in any current version of Chrome, Firefox, Safari, or Edge. Uses `crypto.randomUUID()` where available, with a fallback ID generator for older browsers.

## Known limitations

- Data is stored per-browser, per-device — there's no sync or backend.
- Clearing browser site data / using private browsing will remove saved tasks.
- No undo for delete or "Clear completed" (yet).

## Possible next steps

- Drag-and-drop reordering
- Due dates and sorting
- Export/import tasks as JSON
- Undo toast after delete
