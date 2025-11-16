# Syncing Snapshot Repos with Upstream (git subtree)

This repository stores **snapshots** of external projects (LangChain, LangGraph, DeepAgents, LangSmith, memory frameworks, production agents).

Snapshots are kept in sync with their upstream GitHub repositories using **git subtree**.

`indexed_repos.yaml` is the single source of truth for:

- `local_path` – directory in this repo where the snapshot lives
- `upstream` – GitHub URL
- `branch` – upstream branch to track
- `category` – core-framework | memory-framework | production-agent

> Replace `main` with the actual default branch name (`main` or `master`) if upstream uses a different name.

---

## 1. One-Time Setup: Add Upstream Remotes

From the INDEX repo root (`/Users/Jason/astxrtys/INDEX`):

```bash
# Core frameworks
git remote add langchain-upstream https://github.com/langchain-ai/langchain
git remote add langgraph-upstream https://github.com/langchain-ai/langgraph
git remote add deepagents-upstream https://github.com/langchain-ai/deepagents

# (Optional) others, matching indexed_repos.yaml
# git remote add langsmith-docs-upstream  https://github.com/langchain-ai/langsmith-docs.git
# git remote add langsmith-sdk-upstream   https://github.com/langchain-ai/langsmith-sdk.git
# git remote add cognee-upstream         https://github.com/cognee/cognee.git
# git remote add langmem-upstream        https://github.com/langchain-ai/langmem.git
# git remote add openmemory-upstream     https://github.com/openmemory-ai/OpenMemory.git
# git remote add enterprise-dr-upstream  https://github.com/langchain-ai/enterprise-deep-research.git
# git remote add open-dr-upstream        https://github.com/langchain-ai/open_deep_research.git
# git remote add local-dr-upstream       https://github.com/langchain-ai/local-deep-researcher.git
# git remote add open-swe-upstream       https://github.com/langchain-ai/open-swe.git
```

Run each `git remote add` once. After that, you only need `git subtree` commands with these remotes.

---

## 2. Bootstrapping Existing Snapshots to Use Subtree

Originally, snapshot directories were copied directly (no subtree history). To use `git subtree` going forward, **rewrite each snapshot directory once via subtree**.

### 2.1 Precondition

Ensure the working tree is clean:

```bash
git status
```

If there are uncommitted changes in any snapshot directory you care about, back them up or commit them before proceeding (they will be replaced by the subtree import).

### 2.2 Template for Bootstrapping a Snapshot

For a given snapshot (e.g., `LOCAL_PATH` using remote `REMOTE_NAME` on branch `BRANCH`):

```bash
# 1) Remove the existing snapshot
rm -rf LOCAL_PATH

git add -A
git commit -m "chore: remove existing snapshot at LOCAL_PATH"

# 2) Re-add via subtree from upstream
git subtree add \
  --prefix=LOCAL_PATH \
  REMOTE_NAME \
  BRANCH \
  --squash

git commit -m "feat: add snapshot for LOCAL_PATH via subtree"
```

After this one-time bootstrap, that directory has a clean subtree history tied to its upstream repo.

### 2.3 Concrete Examples (Core Frameworks)

Replace `BRANCH` with the upstream default branch (check GitHub to confirm `main` vs `master`).

**LangChain**

```bash
rm -rf langchain-master
git add -A
git commit -m "chore: remove existing langchain snapshot"

git subtree add \
  --prefix=langchain-master \
  langchain-upstream \
  BRANCH \
  --squash

git commit -m "feat: add langchain snapshot via subtree"
```

**LangGraph**

```bash
rm -rf langgraph-main
git add -A
git commit -m "chore: remove existing langgraph snapshot"

git subtree add \
  --prefix=langgraph-main \
  langgraph-upstream \
  BRANCH \
  --squash

git commit -m "feat: add langgraph snapshot via subtree"
```

**DeepAgents**

```bash
rm -rf deepagents-master
git add -A
git commit -m "chore: remove existing deepagents snapshot"

git subtree add \
  --prefix=deepagents-master \
  deepagents-upstream \
  BRANCH \
  --squash

git commit -m "feat: add deepagents snapshot via subtree"
```

---

## 3. Regular Sync After Bootstrap

Once a directory has been imported via `git subtree add`, syncing to the latest upstream is:

```bash
git subtree pull \
  --prefix=LOCAL_PATH \
  REMOTE_NAME \
  BRANCH \
  --squash
```

### 3.1 Examples (Core Frameworks)

```bash
# LangChain
git subtree pull \
  --prefix=langchain-master \
  langchain-upstream \
  BRANCH \
  --squash

# LangGraph
git subtree pull \
  --prefix=langgraph-main \
  langgraph-upstream \
  BRANCH \
  --squash

# DeepAgents
git subtree pull \
  --prefix=deepagents-master \
  deepagents-upstream \
  BRANCH \
  --squash
```

Use conventional commit messages when updating snapshots, for example:

```bash
git commit -am "feat: update langchain snapshot to v0.x.x"
```

---

## 4. Relationship to indexed_repos.yaml

`indexed_repos.yaml` mirrors exactly what this file describes. For each entry:

- `name` can be mapped to a remote name (e.g., `langchain` → `langchain-upstream`).
- `local_path` is the `--prefix` used in `git subtree add/pull`.
- `upstream` is the remote URL you pass to `git remote add`.
- `branch` is the branch argument in `git subtree add/pull`.
- `category` is informational (core-framework, memory-framework, production-agent).

You can drive automated sync tooling off `indexed_repos.yaml` by iterating its entries and issuing the corresponding `git remote` / `git subtree` commands.
