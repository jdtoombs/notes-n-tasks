# notes-n-tasks

A fullscreen TUI for Markdown notes, tasks, timers, and date-based planning.

Built with [Bubble Tea](https://github.com/charmbracelet/bubbletea), [Lip Gloss](https://github.com/charmbracelet/lipgloss), and [Glamour](https://github.com/charmbracelet/glamour).

## Install

Requires Go 1.24+ and Neovim (`nvim`) for editing notes.

```sh
go build -o notes-n-tasks .
```

## Usage

```sh
./notes-n-tasks
```

Or run directly without building:

```sh
go run .
```

Tasks are stored in `~/.tasks.json`. Notes are stored as Markdown files in `~/.notes-n-tasks/notes` by default. If an existing legacy `~/.task-buddy/notes` directory is found, it is used instead to avoid orphaning existing notes. Override the notes location with `NOTES_N_TASKS_NOTES_DIR=/path/to/notes`; `TASK_BUDDY_NOTES_DIR` is also supported as a legacy override.

The app starts in the work list; existing tasks without a saved list are treated as personal. Incomplete tasks from past dates are automatically carried forward to today on launch.

## Features

- Vim-style navigation with date-based task filtering
- Separate work and personal task lists, toggled with `p`
- Timer tracking with start/stop per task
- Manual time entry (e.g. `1h30m`, `45m`, `1:30`)
- Carry-forward of incomplete past tasks to today
- Desktop notifications for tasks with `@HH:MMam/pm` in the title
- Global notes mode with folder navigation, Markdown notes, Neovim editing, and Glamour-rendered previews

## Keybindings

| Key | Action |
|-----|--------|
| `j` / `k` / arrows | Move cursor up/down |
| `h` / `l` / left/right | Navigate days |
| `t` | Jump to today |
| `g` / `G` | Jump to top/bottom |
| `+` / `o` | Add task |
| `i` | Edit task name |
| `enter` / `space` | Toggle done |
| `s` | Start/stop timer |
| `T` | Add time manually |
| `p` | Toggle work/personal list |
| `n` | Toggle tasks/notes view |
| `d` / `x` | Delete task (with confirmation) |
| `?` | Toggle help overlay |
| `q` / `ctrl+c` | Quit |

### Notes mode

| Key | Action |
|-----|--------|
| `n` | Return to tasks |
| `j` / `k` / arrows | Move cursor up/down |
| `enter` / `l` | Enter folder or edit selected note in `nvim` |
| `h` / `backspace` | Go to parent folder |
| `g` / `G` | Jump to top/bottom |
| `a` | Create Markdown note (`.md` is added when omitted) |
| `A` | Create folder |
| `d` / `x` | Delete file/folder; non-empty folders show a recursive-delete warning |
| `r` | Refresh notes listing and preview |
| `?` | Toggle help overlay |
| `q` / `ctrl+c` | Quit |
