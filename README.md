# ezgit

> Git for humans: simple commands that just work.

<img src="./logo.svg" alt="ezgit logo" width="400" />

[![Go Report Card](https://goreportcard.com/badge/github.com/AndrewDonelson/ezgit)](https://goreportcard.com/report/github.com/AndrewDonelson/ezgit)
[![GoDoc](https://godoc.org/github.com/AndrewDonelson/ezgit?status.svg)](https://godoc.org/github.com/AndrewDonelson/ezgit)
[![License](https://img.shields.io/github/license/AndrewDonelson/ezgit.svg)](https://github.com/AndrewDonelson/ezgit/blob/main/LICENSE)

## Overview

`ezgit` simplifies Git's complex command structure into intuitive, easy-to-remember commands. It's designed for developers who want to focus on coding rather than remembering Git's arcane syntax.

Built as a hybrid implementation, ezgit primarily uses a pure Go Git library (go-git) so it works out-of-the-box without requiring Git installation, while also offering the option to leverage the full Git CLI for advanced operations when available.

## Installation

```bash
# Using Go
go install github.com/AndrewDonelson/ezgit@latest

# Or download the binary for your platform from Releases
```

> **Note**: ezgit works standalone without requiring Git to be installed, but will utilize your system's Git installation for certain advanced operations when available.

## Key Features

- **Intuitive commands** - Commands that describe what they do in plain English
- **Smart defaults** - Less typing, more doing
- **Guided workflows** - Step-by-step assistance for complex operations
- **Intelligent conflict resolution** - Makes merging and rebasing less painful
- **Clear error messages** - Understand what went wrong and how to fix it
- **Visual feedback** - See your repository structure at a glance
- **No dependencies** - Works without requiring Git installation
- **Full power when needed** - Leverages system Git when available for advanced operations

## Usage Examples

### Basic Operations

```bash
# Initialize a repository
ezgit start

# Clone a repository
ezgit clone https://github.com/username/repo.git

# Stage and commit changes (with auto-generated message if omitted)
ezgit save "Add login functionality"

# Pull changes and push commits in one step
ezgit sync
```

### Branch Management

```bash
# Switch to a branch (creates if needed)
ezgit switch feature-login

# List all branches with status information
ezgit branches

# Rename current branch
ezgit rename better-feature-name

# Push branch to remote
ezgit publish
```

### History & Changes

```bash
# Show uncommitted changes
ezgit changes

# Undo last commit
ezgit undo

# Modify last commit
ezgit amend "Fix typo in previous commit"

# Combine last 3 commits into one
ezgit squash 3
```

### Merge & Rebase Operations

```bash
# Merge branch into current
ezgit merge feature

# Rebase current branch onto main
ezgit rebase main

# Rebase feature branch onto main
ezgit rebase feature main
```

### Conflict Resolution

```bash
# Resolve conflicts interactively
ezgit fix

# Continue after resolving conflicts
ezgit continue

# Abort current operation
ezgit abort
```

### Temporary Storage

```bash
# Save changes for later
ezgit park "WIP: Refactoring auth module"

# Restore parked changes
ezgit resume
```

### Collaboration

```bash
# Create pull request
ezgit pr create "Add user authentication"

# List open pull requests
ezgit pr list

# Check out a specific PR
ezgit pr checkout 42
```

### Visualization

```bash
# Show branch history as a visual graph
ezgit graph

# Show who changed each line and when
ezgit blame config.go
```

## Complete Command Reference

| Command | Description |
|---------|-------------|
| `start` | Initialize a repository |
| `clone <url>` | Clone a repository |
| `save ["message"]` | Stage and commit changes |
| `sync` | Pull changes and push commits |
| `fetch` | Update remote information |
| `switch <branch>` | Switch to branch (creates if needed) |
| `branches` | List all branches with status |
| `rename <new-name>` | Rename current branch |
| `delete <branch>` | Delete a branch |
| `publish [branch]` | Push branch to remote |
| `track <remote/branch>` | Set upstream for current branch |
| `changes` | Show uncommitted changes |
| `unstage [file]` | Remove files from staging |
| `discard [file]` | Discard uncommitted changes |
| `diff [branch1] [branch2]` | Compare branches |
| `history [search-term]` | Show commit history |
| `undo [n]` | Undo last n commits |
| `amend ["message"]` | Modify last commit |
| `squash <n>` | Combine last n commits |
| `rewrite` | Interactive history rewriting |
| `pick <commit> [branch]` | Cherry-pick commit |
| `merge <branch>` | Merge branch into current |
| `rebase [source] <target>` | Rebase branch |
| `continue` | Continue after resolving conflicts |
| `abort` | Abort current operation |
| `fix [file]` | Resolve conflicts interactively |
| `fix --ours [file]` | Resolve using our changes |
| `fix --theirs [file]` | Resolve using their changes |
| `park ["label"]` | Save changes for later |
| `resume [index]` | Restore parked changes |
| `parks` | List all parked changes |
| `pr create [title]` | Create pull/merge request |
| `pr list` | List open pull requests |
| `pr checkout <id>` | Check out a specific PR |
| `pr review <id>` | Show PR details/changes |
| `recover` | Suggest fixes for common errors |
| `clean` | Clean up repository |
| `verify` | Check repository health |
| `reflog` | Show reference history |
| `graph` | Show branch history as a graph |
| `blame <file>` | Show who changed each line |
| `tag <name> [commit]` | Create tag |
| `bisect start/good/bad` | Find which commit introduced a bug |
| `submodule <add/update>` | Manage submodules |
| `worktree <add/list/remove>` | Manage worktrees |
| `config` | Set common configurations |

## Why ezgit?

- **For beginners**: No more memorizing cryptic Git commands
- **For teams**: Standardize Git workflows and reduce errors
- **For everyone**: Spend less time on version control, more time coding
- **For environments without Git**: Use version control even where Git can't be installed
- **For portable workflows**: Same commands work across all platforms

## Technical Implementation

ezgit uses a hybrid approach:

- Primary implementation using [go-git](https://github.com/go-git/go-git), a pure Go implementation of Git
- Fallback to system Git (when available) for advanced operations not fully supported by go-git
- Smart detection to use the best implementation for each command

This approach provides the best of both worlds:
1. Works out-of-the-box without Git installation
2. Utilizes the full power of Git when available
3. Consistent command interface regardless of the underlying implementation

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.