# Development Guide for ezgit

This document outlines the development approach, folder structure, and contribution guidelines for the ezgit project.

## Project Structure

```
ezgit/
├── cmd/
│   └── ezgit/
│       └── main.go             # Application entry point
├── internal/
│   ├── core/                   # Core functionality
│   │   ├── git.go              # Git interface definition
│   │   ├── commands.go         # Command interface definitions
│   │   └── context.go          # Repository context
│   ├── gogit/                  # go-git implementation
│   │   ├── repository.go       # go-git repository wrapper
│   │   ├── basic.go            # Basic operations (clone, init, etc.)
│   │   ├── branch.go           # Branch operations
│   │   ├── commit.go           # Commit operations
│   │   └── worktree.go         # Worktree operations
│   ├── gitcli/                 # Git CLI fallback implementation
│   │   ├── repository.go       # Git CLI repository wrapper
│   │   ├── commands.go         # Git CLI command execution
│   │   └── advanced.go         # Advanced operations
│   ├── hybrid/                 # Hybrid implementation (combining go-git and Git CLI)
│   │   ├── repository.go       # Hybrid repository implementation
│   │   └── selector.go         # Implementation selector
│   ├── util/                   # Shared utilities
│   │   ├── fs.go               # Filesystem utilities
│   │   ├── output.go           # Output formatting
│   │   └── validation.go       # Input validation
│   └── ui/                     # User interface components
│       ├── color.go            # Terminal color support
│       ├── progress.go         # Progress indicators
│       └── prompt.go           # Interactive prompts
├── pkg/                        # Public packages for potential reuse
│   ├── constants/              # Public constants
│   │   └── version.go          # Version information
│   └── models/                 # Public data models
│       ├── branch.go           # Branch representation
│       ├── commit.go           # Commit representation
│       └── repository.go       # Repository representation
├── scripts/                    # Build and development scripts
│   ├── build.sh                # Build script
│   └── test.sh                 # Test script
├── docs/                       # Documentation
│   ├── commands/               # Command documentation
│   ├── examples/               # Usage examples
│   └── diagrams/               # Architecture diagrams
├── .github/                    # GitHub configuration
│   ├── workflows/              # GitHub Actions workflows
│   │   ├── build.yml           # Build workflow
│   │   └── test.yml            # Test workflow
│   └── ISSUE_TEMPLATE/         # Issue templates
├── .gitignore                  # Git ignore file
├── go.mod                      # Go module file
├── go.sum                      # Go module checksum
├── LICENSE                     # License file
├── README.md                   # Project README
└── DEVELOPMENT.md              # This file
```

## Layered Implementation

ezgit is built using a layered approach, from simplest to most advanced:

### Layer 1: Core Interfaces and Utilities

The foundation of ezgit consists of core interfaces and utility functions that define the abstractions for Git operations and provide common functionality:

- **Core Interfaces**: Define the contract for Git operations
- **Utilities**: Provide common functionality for file handling, output formatting, etc.
- **Models**: Define the data structures used throughout the application

### Layer 2: Implementation Providers

ezgit uses two primary implementation providers:

1. **go-git Provider**: Pure Go implementation using the go-git library
   - Implements basic operations (clone, init, commit, branch, etc.)
   - Works without external dependencies

2. **Git CLI Provider**: Fallback implementation using the system Git CLI
   - Provides access to advanced Git features
   - Requires Git to be installed on the system

### Layer 3: Hybrid Implementation

The hybrid layer combines the go-git and Git CLI providers:

- **Implementation Selector**: Chooses the appropriate provider based on:
  - Command complexity
  - Available implementations
  - User preferences
  - System capabilities

- **Repository**: Provides a unified interface to the underlying Git operations

### Layer 4: Command Interface

The command interface translates user-friendly commands into the appropriate Git operations:

- Maps intuitive commands to Git operations
- Handles parameter parsing and validation
- Provides helpful error messages and suggestions
- Implements smart defaults and context-aware behavior

### Layer 5: User Interface

The user interface layer provides a user-friendly CLI experience:

- Formatted output with color support
- Progress indicators for long-running operations
- Interactive prompts for conflict resolution
- Help and documentation

## Development Workflow

### Setting Up Development Environment

1. Clone the repository:
   ```bash
   git clone https://github.com/AndrewDonelson/ezgit.git
   cd ezgit
   ```

2. Install dependencies:
   ```bash
   go mod download
   ```

3. Build the project:
   ```bash
   go build -o bin/ezgit ./cmd/ezgit
   ```

### Adding a New Command

1. Define the command interface in `internal/core/commands.go`
2. Implement the command in both providers:
   - `internal/gogit/` for the go-git implementation
   - `internal/gitcli/` for the Git CLI implementation
3. Add the hybrid implementation in `internal/hybrid/`
4. Update the command interface in the main application
5. Add tests for the new command
6. Update documentation

### Running Tests

Run tests with:

```bash
go test ./...
```

Or use the test script:

```bash
./scripts/test.sh
```

### Building

Build the project with:

```bash
go build -o bin/ezgit ./cmd/ezgit
```

Or use the build script:

```bash
./scripts/build.sh
```

## Implementation Details

### Core Interfaces

The core interfaces define the contract for Git operations:

```go
// Repository represents a Git repository
type Repository interface {
    // Basic operations
    Init() error
    Clone(url string, options CloneOptions) error
    
    // Branch operations
    Branches() ([]Branch, error)
    CreateBranch(name string) error
    SwitchBranch(name string) error
    
    // ... other operations
}
```

### Implementation Providers

#### go-git Provider

The go-git provider implements the core interfaces using the go-git library:

```go
type GoGitRepository struct {
    repo *git.Repository
    path string
}

func (r *GoGitRepository) Init() error {
    // Implementation using go-git
}

// Other implementations...
```

#### Git CLI Provider

The Git CLI provider implements the core interfaces by executing Git commands:

```go
type GitCLIRepository struct {
    path string
}

func (r *GitCLIRepository) Init() error {
    // Implementation using Git CLI
}

// Other implementations...
```

### Hybrid Implementation

The hybrid implementation selects the appropriate provider:

```go
type HybridRepository struct {
    gogit *GoGitRepository
    gitcli *GitCLIRepository
    path string
}

func (r *HybridRepository) Init() error {
    // Choose the appropriate implementation
    return r.gogit.Init()
}

func (r *HybridRepository) RebaseInteractive(target string) error {
    // Use Git CLI for advanced operations
    return r.gitcli.RebaseInteractive(target)
}

// Other implementations...
```

## Contribution Guidelines

### Code Style

- Follow standard Go coding conventions
- Use gofmt to format code
- Follow the [Effective Go](https://golang.org/doc/effective_go) guidelines

### Commit Messages

Follow the [Conventional Commits](https://www.conventionalcommits.org/) format:

```
<type>(<scope>): <description>

[optional body]

[optional footer(s)]
```

Types:
- `feat`: A new feature
- `fix`: A bug fix
- `docs`: Documentation only changes
- `style`: Changes that do not affect the meaning of the code
- `refactor`: A code change that neither fixes a bug nor adds a feature
- `test`: Adding missing tests or correcting existing tests
- `chore`: Changes to the build process or auxiliary tools

### Pull Request Process

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'feat: add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Testing Requirements

- Add tests for new functionality
- Ensure all tests pass before submitting a pull request
- Maintain or improve code coverage

## Performance Considerations

- Prefer go-git for most operations to avoid subprocess overhead
- Use Git CLI for operations where performance is critical
- Cache repository information to avoid repeated lookups
- Minimize filesystem operations

## Documentation

- Update documentation when adding or changing features
- Provide examples for new commands
- Keep the README up to date

## Release Process

1. Update version in `pkg/constants/version.go`
2. Update CHANGELOG.md
3. Tag the release (`git tag v1.0.0`)
4. Push the tag (`git push origin v1.0.0`)
5. GitHub Actions will build and publish the release
