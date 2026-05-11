# torchao_ci_xpu

A lightweight helper repository that lets contributors request XPU CI runs on
[`pytorch/ao`](https://github.com/pytorch/ao) PRs **without needing write access
to `pytorch/ao`**. Users open an issue here (or add a comment), and a GitHub
Actions workflow attaches the appropriate `ciflow/*` label to the target PR.

## How it works

```
contributor                         this repo                  pytorch/ao
    │                                   │                          │
    │  open issue (PR #, labels)        │                          │
    ├──────────────────────────────────►│                          │
    │                                   │  parse + authorize       │
    │                                   ├──── add label ──────────►│
    │                                   │                          │
    │  ◄────── comment + close ─────────┤                          │
```

The workflow runs in this repo with a Personal Access Token (PAT) or GitHub App
token that has `pull-requests: write` on `pytorch/ao`.

## Usage

### Option 1: Open an issue (recommended)

1. Click **New Issue** → **Add ciflow label**.
2. Fill in the form:
   - **PR number** in `pytorch/ao` (e.g. `1234`)
   - **Labels** to add, comma-separated (e.g. `ciflow/xpu`)
3. Submit. The bot will:
   - Add the labels to the target PR
   - Comment on the issue with the result
   - Close the issue

### Option 2: Comment on an existing issue

Post a comment in the form:

```
/add-label <pr_number> <label1>[,<label2>...]
```

Example:

```
/add-label 1234 ciflow/xpu
```

## Allowed labels

Only labels in the allow-list are accepted (configured in
`.github/workflows/add-ciflow-label.yml`):

- `ciflow/xpu`
- `ciflow/xpu-periodic`
- `topic: xpu`
- `module: not user facing`

To extend the list, edit `ALLOWED_LABELS` in the workflow file.

## Authorization

By default, anyone can open an issue, but only users in the allow-list (org
members of `pytorch` or explicit collaborators listed in the workflow) can
trigger labeling. Unauthorized requests are rejected with a comment.

## Secrets

The workflow expects one repository secret:

| Secret           | Description                                                     |
| ---------------- | --------------------------------------------------------------- |
| `TORCHAO_PAT`    | PAT or GitHub App installation token with `pull-requests: write` on `pytorch/ao`. |

## Files

```
.github/
├── ISSUE_TEMPLATE/
│   └── add-ciflow-label.yml      # Issue form template
└── workflows/
    └── add-ciflow-label.yml      # The labeling workflow
```
