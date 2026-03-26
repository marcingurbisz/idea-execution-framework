# Easier start plan for IEF

Date: 2026-03-16

## Trigger

Feedback from a colleague:

> Przydałby się jakiś boilerplate maker :D strasznie się namęczyłem z simlinkami i kontenerem 😛
> Jakiś npx ief init-project "nazwa"

## Diagnosis

The main onboarding problem is not just “missing tooling”.
It is that the current README exposes the more advanced workspace-level setup early enough that a new user can mistake it for the default path.

That creates unnecessary friction:
- symlinks
- workspace-root `.devcontainer`
- multi-repo assumptions

For many users, the real desired first step is much simpler:
- one repo
- one `AGENTS.md`
- one `README.md`
- one queue surface (`TODO.md` or `todo/` topic files)
- start the agent and run the loop

## Recommended staged fix

### Stage 1 — README-first improvement

Make the simple path explicit and put it before the advanced workspace setup.

This is the highest-leverage immediate fix because:
- it helps today
- it is cheap
- it reduces the chance that users start with the hardest path first

### Stage 2 — init helper spec

If onboarding friction still matters after the README improvement, add a small bootstrap helper.

Recommended shape:

- `npx ief init-project <name>`
- `npx ief init-workspace <name>`

Why two commands instead of one overloaded command:
- `init-project` matches the simple path
- `init-workspace` makes the advanced path explicit
- avoids making symlinks/devcontainer choices feel like hidden defaults

## What `init-project` should do

Target: the common first-use case.

Outputs:
- `AGENTS.md`
- `README.md`
- either `TODO.md` or starter `todo/` directory
- optional `.gitignore` additions if useful

Behavior:
- no symlinks required
- no multi-repo assumptions
- should work in an existing repo or create a fresh folder

Suggested flags:
- `--queue todo-md`
- `--queue topic-files`
- `--with-devcontainer`
- `--ide copilot|codex|claude`

## What `init-workspace` should do

Target: the advanced multi-repo setup.

Outputs:
- workspace root `AGENTS.md`
- workspace root `.devcontainer` link or generated config
- optional sample repo layout
- clear explanation of how project repos fit underneath

Behavior:
- should be explicit about creating symlinks
- should explain what is being linked and why
- should not be the implied default for first-time users

## Recommendation for naming and UX

Prefer plain commands over clever ones:

- `init-project`
- `init-workspace`

These are self-explanatory and match the two actual onboarding modes.

## Recommendation for priority

Near-term:
1. improve README
2. document the split between simple and advanced setup

Later, if still useful:
3. build `npx` bootstrap helper

## Bottom line

The root cause is mostly default-path clarity, not only missing automation.

So the best immediate improvement is:
- put the single-repo path first
- clearly mark workspace-level setup as advanced
- only then consider an `npx` initializer as the next convenience layer