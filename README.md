# gh-nvim-username-keywords

A [GitHub CLI extension](https://docs.github.com/en/github-cli/github-cli/creating-github-cli-extensions) that adds GitHub usernames to your nvim keywords file for autocomplete. It discovers usernames from your GitHub teams and PR activity, making it easy to @-mention colleagues.

<img width="718" height="261" alt="Example dry-run output" src="https://github.com/user-attachments/assets/a9ff579d-762a-4279-912b-0de62ee8a84f" />


## Installation

Install this GitHub CLI extension using:

```bash
gh extension install joshbeckman/gh-nvim-username-keywords
```

Or with the full URL:

```bash
gh extension install https://github.com/joshbeckman/gh-nvim-username-keywords
```

## Prerequisites

- [GitHub CLI (`gh`)](https://cli.github.com/) must be installed and authenticated
- Neovim with a keywords.txt file at `$XDG_CONFIG_HOME/nvim/keywords.txt` (defaults to `~/.config/nvim/keywords.txt`)

## Usage

```bash
gh nvim-username-keywords [OPTIONS]
```

### Options

| Option | Description |
|--------|-------------|
| `--team ORG/SLUG` | Add a team to query (repeatable) |
| `--repo OWNER/NAME` | Add a repo to query (repeatable) |
| `--limit N` | Override PR limit (default: 30) |
| `--dry-run` | Show what would be added without writing |
| `--verbose, -v` | Show progress |
| `-h, --help` | Show help |

If no `--team` or `--repo` is specified, the extension auto-discovers defaults:
- **Team**: Your most recent team membership
- **Repo**: The repo from your most recent PR activity

### Examples

**Use auto-discovered defaults:**
```bash
gh nvim-username-keywords
```

**Specify teams explicitly:**
```bash
gh nvim-username-keywords --team myorg/engineering --team myorg/design
```

**Specify repos explicitly:**
```bash
gh nvim-username-keywords --repo owner/another-repo
```

**Preview changes without writing:**
```bash
gh nvim-username-keywords --dry-run --verbose
```

## How It Works

The extension collects usernames from two sources:

1. **GitHub Teams**: Members of teams you specify (or auto-discovered from your most recent team membership)
2. **PR Activity**: Authors, reviewers, and commenters from PRs you've authored or reviewed in specified repos (or auto-discovered from your most recent PR)

It then:
- Filters out known bot accounts
- Compares against existing entries in your keywords file
- Appends only new usernames (prefixed with `@`)

The keywords file is append-only—existing entries are never removed.
