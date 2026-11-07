---
name: repository-management
description: Locate and clone GitHub and Overleaf repositories using the user's ghq and directory conventions. Use when obtaining a repository or choosing its local checkout location. GitHub clones require ghq unless the user explicitly specifies an exception; canonical locations are ~/Documents/ghq/GitHub and ~/Documents/ghq/Overleaf. Does not automatically migrate existing checkouts or change global configuration.
---

# Repository management

## Required layout

Resolve paths using the current user's home directory, never a hard-coded username:

```text
~/Documents/ghq/
├── GitHub/<owner>/<repository>/
└── Overleaf/<project>/
```

- Clone GitHub repositories with `ghq`, unless the user explicitly requests a different cloning method. Tool failure, convenience, or missing ghq is not an exception. If ghq is unavailable, report the prerequisite rather than silently using `git clone` or `gh repo clone`.
- Honor an explicit user destination or protocol override for that request. Do not turn an exception into a new default.
- Keep permanent checkouts under `GitHub` or `Overleaf` as appropriate. Do not create permanent `github.com/`, `git.overleaf.com/`, alternate-cased provider directories, or a second `~/ghq` tree for new clones.
- Preserve the owner and repository names for GitHub. For Overleaf, use a known project name; if unavailable, use its project ID. Verify identity by the remote project ID, not the display name alone.
- These rules apply to obtaining and locating repositories. Do not move existing repositories or change global ghq, Git, or SSH configuration merely by loading this skill.

## Locate before cloning

Inspect `ghq root --all`, `ghq list -p`, the canonical destination, and any known legacy clone location. Check a candidate's Git remote to establish that it is the requested repository. Normalize SSH and HTTPS forms for comparison; do not confuse matching directory names with matching remotes.

Reuse a matching existing checkout, including a legacy checkout outside the new layout, rather than creating a duplicate. Report its location and the layout discrepancy. Migration is a separate operation requiring a user request; preserve dirty working trees, worktrees, and existing links.

If the destination exists for another repository or is a non-repository directory, stop that clone and report the collision. Do not overwrite, delete, or merge its contents. Do not update an existing checkout unless the task calls for updating it.

## GitHub: SSH first, through ghq

Use the full GitHub remote and prefer SSH, for example `ghq get -p git@github.com:OWNER/REPOSITORY.git`. Check the installed `ghq get --help` before relying on flags.

ghq normally derives a host-named directory from the remote URL. Setting its root alone does not produce the required `GitHub/<owner>/<repository>` layout. Use this acquisition procedure instead:

1. Determine the final canonical destination and check for existing clones and destination collisions before any acquisition.
2. Create a unique temporary staging directory outside the permanent checkout tree. Run ghq with `GHQ_ROOT` set only for that command to the staging directory. Do not repurpose the shell's `HOME` or write global config.
3. Acquire the exact remote through ghq using SSH. The transient staging tree may contain `github.com`; it is not a permanent checkout location.
4. Verify the acquired checkout's remote identity and top-level directory. Recheck that the final destination is absent, then move the complete checkout into `~/Documents/ghq/GitHub/<owner>/<repository>`, preserving `.git` and all contents. Do not overwrite a destination created concurrently. Clean up only task-owned staging data after successful verification.
5. Verify the final checkout with Git and its remote URL. Use the canonical path for subsequent work. Use ghq listing plus remote verification to locate it in later tasks; do not blindly rerun `ghq get`, which could recreate the host-named layout.

If an SSH attempt fails because of sandbox network, SSH-agent, or keychain restrictions, use the platform's supported approval/escalation mechanism to retry the same intended operation outside the sandbox when permitted. First inspect whether the previous attempt produced a complete or partial clone; do not blindly duplicate an uncertain operation. A sandbox failure alone is not a reason to switch protocols.

If an operation is explicitly denied, do not bypass that denial. Report the action and reason; retry only when materially new authorization permits it. Never disable SSH host-key verification or expose credentials to make cloning work.

If SSH remains unavailable outside the sandbox, HTTPS through ghq is an allowed fallback because SSH is a preference, not a requirement. Explain the fallback. For invalid credentials or an authentication/connection approval prompt, stop the affected action and request the required recovery rather than changing protocol to evade access controls.

## Overleaf

Place newly obtained projects under `~/Documents/ghq/Overleaf/<project>`. Use the project’s actual supported Git transport; do not assume GitHub's SSH endpoint or invent an Overleaf SSH endpoint. ghq may be used with the same temporary-staging procedure and the supported remote URL.

When an Overleaf-specific workflow skill is available, follow it for authentication, synchronization, manuscript edits, validation, and push, while retaining this canonical placement rule unless the user explicitly overrides it. Do not migrate an existing manuscript checkout as a side effect of working on it.

## Completion

Report the final local path, whether an existing checkout was reused, and the protocol used for a new clone. Report any blocker accurately. Repository acquisition does not authorize editing project files, committing, pushing, publishing repositories, installing skills, or changing hooks.
