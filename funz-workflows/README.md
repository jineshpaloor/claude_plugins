# funz-workflows

Claude Code plugin providing PR and code review workflow commands for the **funz**
project. The commands are tailored to a Python codebase.

## Commands

| Command | What it does |
|---|---|
| `/review-pr [PR number or URL]` | Reviews a pull request for correctness, Python idioms, tests, API compatibility, and performance. Outputs findings grouped by severity with a verdict. |
| `/security-review [PR / branch / path]` | Audits changed code for security issues — injection, secrets, auth, input validation, crypto, dependencies. Reports severity, exploit scenario, and remediation. |
| `/refactor <file / function / path>` | Proposes and applies behavior-preserving refactors, then verifies with `pytest` and the repo's linters. |

## Install

From a local checkout:

```bash
claude plugin install ./funz-workflows
```

Or via the bundled marketplace (this repo):

```bash
claude plugin marketplace add /path/to/this/repo
claude plugin install funz-workflows@ambika-marketplace
```

Then restart Claude Code and run any command, e.g. `/review-pr 142`.

## Validate (during development)

```bash
claude plugin validate ./funz-workflows
```

## Layout

```
.
├── .claude-plugin/
│   └── marketplace.json        # marketplace listing for distribution
└── funz-workflows/             # the plugin
    ├── .claude-plugin/
    │   └── plugin.json          # plugin manifest
    ├── commands/
    │   ├── review-pr.md
    │   ├── security-review.md
    │   └── refactor.md
    └── README.md
```

## Adding a command

Drop a new `commands/<name>.md` file in the plugin. The filename becomes the
command (`commands/deploy.md` → `/deploy`). Use frontmatter for `description`,
`argument-hint`, and `allowed-tools`, and `$ARGUMENTS` in the body to capture
what the user types.
