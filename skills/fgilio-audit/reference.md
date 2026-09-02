# Provenance

The three audit prompts in `prompts/` derive from **"Audit your codebase"** by Aaron Francis:

- Gist: https://gist.github.com/aarondfrancis/8735edbe48532f97ee5ea818db4dbd47
- Raw (pinned revision): https://gist.githubusercontent.com/aarondfrancis/8735edbe48532f97ee5ea818db4dbd47/raw/959a2a9c1ed1648f39872885b2686ce5b576e78c/audit-your-codebase.md

| File | Origin |
| --- | --- |
| `prompts/project.md` | Aaron's gist, verbatim. |
| `prompts/branch.md` | Aaron's prompt rescoped to the current branch and its PR: the diff becomes the source of truth for scope; surrounding code may be inspected but never turns into repo-wide cleanup. |
| `prompts/feature.md` | Aaron's prompt rescoped to one feature spanning several repositories (one PR each). Review areas may cross repository boundaries so that mismatched state models, duplicated business rules, unsafe rollout assumptions, and unclear contract ownership are not missed. |

The `branch` and `feature` variants were produced with ChatGPT from Aaron's original. Only edit: the feature variant says "the PRs" instead of "the four PRs", so the repository count is a parameter.
