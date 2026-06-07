# notes-n-tasks

Fullscreen TUI for Markdown notes, tasks, timers, and date-based planning.

## Tech Stack
- Go 1.24+
- Bubble Tea (charmbracelet/bubbletea) - TUI framework
- Lip Gloss (charmbracelet/lipgloss) - terminal styling
- Glamour (charmbracelet/glamour) - Markdown preview rendering
- Neovim (`nvim`) - external editor for notes

## Build & Run
```
go build -o notes-n-tasks .
./notes-n-tasks
```

## Architecture
Root-level `main.go` using the Elm architecture via Bubble Tea, with `store/` package for data types and persistence:
- `model` holds all state: tasks, cursor position, current date, input mode, active work/personal context, active top-level view, and notes browser state
- `store.TaskData` holds the persisted task collection
- `Update()` handles key events, delegates to `updateNormal()` / `updateInsert()` / `updateEdit()` / `updateConfirmDelete()` / `updateTimeEdit()` based on mode
- `View()` routes to tasks or notes rendering; tasks render date header/task list/footer, notes render a two-pane file browser plus Markdown preview
- Modes include normal task navigation, task insert/edit/delete/time entry, help overlay, notes create-file/create-folder, and notes delete confirmation
- Timer support: tracks time per task via `store.TimeEntry`, with start/stop in normal mode
- Manual time entry: `T` key allows adding time in formats like `1h30m`, `45m`, `1:30`
- Carry-forward: on launch, incomplete past tasks are copied to today (originals marked done), tracked via `CarriedFromID` to prevent duplicates
- Reminder system: tasks with `@HH:MMam/pm` in the title trigger WSL notifications when due; automatic reminders remain active while `n` is used for the notes toggle

## Data
- Tasks persist to `~/.tasks.json` as a flat JSON array
- Notes persist as Markdown files and folders under `~/.notes-n-tasks/notes` by default; an existing legacy `~/.task-buddy/notes` directory is used when present; `NOTES_N_TASKS_NOTES_DIR` overrides this location, with `TASK_BUDDY_NOTES_DIR` supported as a legacy override
- Each task has: id, title, done (bool), date (YYYY-MM-DD), context (work/personal), notified (bool), entries (time tracking), carried_from_id (optional, tracks carry-forward origin)
- Tasks are filtered by the currently viewed date and active context (work/personal)
- Missing/empty task context is treated as personal for existing data; app starts in work context
- Time entries track start/end timestamps per task for timer functionality
- Notes mode uses safe path resolution under the notes root and blocks path traversal outside that root

## Keybindings
- `j/k` or arrows: move cursor
- `h/l` or left/right: navigate days
- `t`: jump to today
- `g/G`: jump to top/bottom
- `+` or `o`: add task (enter insert mode)
- `i`: edit task name
- `enter` or `space`: toggle done
- `s`: start/stop timer on selected task
- `T`: add time manually (e.g. 1h30m, 45m, 1:30)
- `p`: toggle work/personal list
- `n`: toggle tasks/notes view
- `d` or `x`: delete task (with confirmation)
- `?`: toggle help overlay
- `q` or `ctrl+c`: quit

Notes mode:
- `n`: return to tasks
- `j/k` or arrows: move cursor
- `enter` or `l`: enter folder or edit note in `nvim`
- `h` or `backspace`: parent folder
- `a`: create Markdown note (`.md` added by default)
- `A`: create folder
- `d` or `x`: delete file/folder; non-empty folders show a recursive warning before removal
- `r`: refresh listing/preview

## Limitations
- No file locking: concurrent instances can overwrite each other's changes
- Notifications are WSL-specific (uses `powershell.exe`)
