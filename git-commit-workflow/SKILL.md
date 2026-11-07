---
name: git-commit-workflow
description: Apply the user's commit message and automatic commit rules when an AI changes files in a Git repository. Use for creating commits or completing file-changing tasks; covers required type and scope, English titles, structured bodies, AI attribution, and protecting existing changes. Does not manage cloning, repository placement, push, or hooks.
---

# Git commit workflow

## When to commit

- For an authorized file-changing task in a Git repository, commit the AI's changes after appropriate verification and before the final response. Split independent changes into meaningful commits when useful; do not commit after every tool call or intermediate edit.
- Respect an explicit instruction to wait, plan only, or leave changes uncommitted.
- Confirm the target is inside a Git working tree. Outside one, skip committing; do not initialize a repository merely to satisfy this skill.
- Skip commits for read-only work or when no actual changes remain. Never use `--allow-empty`.
- Commit and push are separate actions. This skill does not authorize push.

## Isolate the changes

Inspect the working tree and index before editing so pre-existing staged, unstaged, and untracked changes can be distinguished from this task's work. Before committing, review the exact staged diff and confirm it contains only the intended changes.

Stage explicit paths or selected hunks. Do not sweep unrelated changes into the commit. Preserve pre-existing staged content and user changes; do not reset, discard, stash, or rewrite them just to make committing easier. If ownership cannot be established or changes cannot safely be separated, leave them intact and report why the commit was not made.

Do not amend or rewrite existing commits unless requested. If a commit fails, inspect the cause and report or resolve it within the task's scope; do not bypass hooks or create an empty commit to force completion.

## Message format

```text
type(scope): English one-line summary

## 🛠️ What changed
- Concrete changes, written in the repository's language.

## 💡 Why
Purpose or reason, written in the repository's language.

## 🧪 Validation
- Checks actually performed and their results, when applicable.

Co-authored-by: AI name <AI email>
```

- Both `type` and `scope` are required. Use lowercase for both.
- The entire title must be English. Its description after `: ` is the one-line summary; do not add a separate summary heading or repeat it as an introductory paragraph.
- `## 🛠️ What changed` and `## 💡 Why` are mandatory, even for small changes. Each may be a single concise sentence or bullet.
- Include `## 🧪 Validation` when verification was performed. State the actual results, including failures or limitations; do not claim unperformed checks passed.
- Keep these headings in English with the exact icons above. Write explanations in the language required by repository instructions; otherwise follow established commit-body language, then repository documentation language. Use Japanese if no convention can be established.
- Separate the title, sections, and final trailer block with blank lines.
- For an intentional breaking change, use `type(scope)!: summary` and a `BREAKING CHANGE: explanation` footer describing the impact and required migration.

## Choose the type

| Type | Purpose |
| --- | --- |
| `ai` | Add, change, or remove AI instructions or configuration: `AGENTS.md`/`AGENT.md`, prompts, AI configuration, or Skill files including their dedicated scripts and references. |
| `feat` | Add a capability or feature. |
| `fix` | Correct faulty behavior or incorrect content. |
| `docs` | Change human-facing documentation. |
| `refactor` | Restructure without changing external behavior. |
| `perf` | Improve speed or resource usage. |
| `test` | Add or change tests. |
| `build` | Change dependencies or build configuration. |
| `ci` | Change CI/CD configuration. |
| `style` | Change formatting or whitespace without changing meaning. |
| `chore` | Maintenance that does not fit another type. |
| `revert` | Revert a previous commit. |

Use `ai` in preference to `feat`, `fix`, `docs`, or other types when the commit's purpose is changing AI instructions or Skill behavior. Make the action clear in the English summary, such as `add`, `fix`, `clarify`, or `remove`. An incidental AI-file change in a broader task does not automatically change the task's primary purpose.

Do not use `add` as a type: classify what is being added. Use the primary purpose for one coherent change, including its supporting tests and documentation. Do not use `chore` merely because classification takes thought.

In a skills repository, changes to a Skill use `ai(<skill-directory-name>)`. Human-facing repository documentation uses `docs(repo)`; repository-wide validation CI uses `ci(repo)`.

## Choose the scope

- Use a short target name made of lowercase English letters, digits, and hyphens. Do not use spaces, paths, or comma-separated lists.
- Prefer the repository's established scope names. Otherwise use a stable feature, component, or directory name, such as `auth`, `api`, or `parser`.
- In a skills repository, use the Skill directory name, such as `git-commit-workflow`.
- Use `repo` for repository-wide changes. For shared infrastructure, use the name of that infrastructure when it identifies the target more precisely.
- For an inseparable change spanning several targets, use their shared parent area, or `repo` if none exists. Split changes with independent purposes.
- Do not use action names such as `update` or vague scopes such as `misc` or `other`.

## AI attribution

Every AI-created commit must end with attribution for the AI that actually did the work. Maintain the following mapping as additional AI identities are confirmed:

| AI | Exact trailer |
| --- | --- |
| Codex | `Co-authored-by: Codex <noreply@openai.com>` |

Do not infer the AI's identity solely from an underlying model family. For an unmapped AI, obtain its confirmed name and email before committing; do not invent an address or substitute Codex. Extend the table only when requested or confirmed. No other AI identities are defined yet.

Include each applicable trailer exactly once; preserve other valid attribution trailers. Keep the user's existing Git author/committer configuration. The co-author trailer records AI participation without changing that configuration.

## Verify and report

After committing, inspect the resulting commit's message and file changes. Confirm required type/scope, English title, required sections, correct attribution, and absence of unrelated changes. Report the commit hash and any verification limitations in the final response. When skipping a commit, briefly state the relevant reason.

Cloning, ghq placement, GitHub repository creation, Skill distribution, Notion integration, and hooks belong to separate workflows. Do not configure them through this skill.
