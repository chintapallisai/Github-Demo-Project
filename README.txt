# Team Task Manager — Git & GitHub Hands-on Project

A small Python project for practising Git, GitHub branches, fetch/pull/push, Pull Requests, code review, tests, and merge conflicts.

## Prerequisites
- Git installed (`git --version`)
- Python 3.10+ (`python --version` or `py --version`)
- A GitHub account
- Git configured with your name and email

Configure Git once:
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
```

## Project files
- `task_manager.py` — tiny command-line task manager
- `test_task_manager.py` — automated unit tests
- `requirements.txt` — no third-party dependencies needed
- `.gitignore` — excludes Python cache and virtual environments

## Part 1 — Create a GitHub repository and push the starter project

1. On GitHub, create a **new empty repository** named `team-task-manager-git-demo`.
2. Do not add a README, license, or `.gitignore` on GitHub, because this folder already has files.
3. Open a terminal in this folder and run:

```bash
git init -b main
git status
git add .
git status
git commit -m "Initial task manager application"
git log --oneline
git remote add origin https://github.com/YOUR-USERNAME/team-task-manager-git-demo.git
git remote -v
git push -u origin main
```

Replace `YOUR-USERNAME` with your GitHub username. If your GitHub repo uses SSH, use its SSH URL instead.

What happened:
- `git init -b main`: created a local repository with a `main` branch.
- `git status`: showed tracked/untracked/modified files.
- `git add .`: staged current changes.
- `git commit`: saved a snapshot in local Git history.
- `git remote add origin`: connected the local repo to GitHub.
- `git push -u origin main`: uploaded `main` and set its upstream.

## Part 2 — Clone it as a second working copy

This simulates a second developer's computer. Move to the parent folder first:

```bash
cd ..
git clone https://github.com/YOUR-USERNAME/team-task-manager-git-demo.git developer-b
cd developer-b
git status
git branch -a
git remote -v
```

Now you have two working copies:
- Original folder: Developer A
- `developer-b`: Developer B

Use separate terminal windows for each folder to make the simulation easier.

## Part 3 — Developer A creates a feature branch

In Developer A's original folder:

```bash
git switch main
git pull origin main
git switch -c feature/add-priority
git branch
```

Edit `task_manager.py` and add a `priority` field to task creation, or use the sample feature instructions in `EXERCISES.md`.

Then run and test:
```bash
python task_manager.py
python -m unittest -v
git status
git diff
git add task_manager.py test_task_manager.py
git diff --staged
git commit -m "Add task priority"
git log --oneline --graph --all
git push -u origin feature/add-priority
```

On Windows, if `python` is not recognized, try `py` instead.

## Part 4 — Developer B fetches remote updates

In the `developer-b` folder:

```bash
git fetch origin
git branch -a
git log --oneline --graph --all
```

`fetch` downloads remote branch and commit information but does **not** merge it into the current local branch.

To inspect a remote branch:
```bash
git log origin/feature/add-priority --oneline
```

To update local `main`:
```bash
git switch main
git pull origin main
```

`pull` fetches remote changes and integrates them into the current branch. It does not mean “merge my feature into GitHub main.”

## Part 5 — Open a Pull Request (PR)

After Developer A pushes `feature/add-priority`:

1. Open the GitHub repository.
2. Choose **Pull requests → New pull request**.
3. Base: `main`; compare: `feature/add-priority`.
4. Inspect the **Files changed** tab.
5. Create the PR with a useful title and description.
6. Review comments and checks.
7. When approved and checks pass, click **Merge pull request** (if the repo allows it).

A PR is a GitHub collaboration/review request, not a Git command. Pushing a branch does not itself change `main`.

## Part 6 — Make a second change and practise pull

After the first PR is merged, in Developer B's folder:

```bash
git switch main
git fetch origin
git status
git pull origin main
git log --oneline --graph --all
```

Now `developer-b`'s local `main` includes the merged change. Create another branch:
```bash
git switch -c bugfix/task-status
```

Make a fix, run tests, commit, push, and open a second PR.

## Part 7 — Practise updating a feature branch with main

While working on a feature branch, another PR may merge into `main`. Update your branch like this:

```bash
git fetch origin
git switch feature/add-priority
git merge origin/main
```

If a conflict occurs, Git will mark the conflicted file. Open it, choose the correct combined content, remove conflict markers, then:
```bash
git status
git add <resolved-file>
git commit -m "Resolve merge conflict with main"
git push
```

The exact resolution depends on the conflict. Re-run tests before updating the PR.

## Part 8 — Useful commands cheat sheet

```bash
git status                     # current state
git branch                     # local branches
git branch -a                  # local and remote-tracking branches
git switch main                # switch branches
git switch -c feature/name     # create and switch
git add file.py                # stage a file
git add .                      # stage current changes (review first)
git diff                       # unstaged changes
git diff --staged              # staged changes
git commit -m "message"        # create commit
git log --oneline --graph --all # visual history
git fetch origin               # download remote updates, don't integrate
git pull origin main           # integrate remote main into current branch
git push -u origin branch-name # push branch and set upstream
git merge origin/main          # merge remote-tracking main into current branch
git remote -v                  # show remotes
git restore file.py            # discard unstaged changes to a file (careful!)
git restore --staged file.py   # unstage a file, keep edits
```

## Common cases
- **Push rejected / non-fast-forward:** fetch and integrate the remote changes, resolve conflicts if needed, then push again.
- **Tests fail on PR:** investigate, fix on the same branch, commit and push; the PR updates automatically.
- **Merge conflict:** edit the file to the desired final content, remove conflict markers, stage, commit, retest.
- **Wrong branch:** check `git status` and `git branch` before editing or committing.
- **Do not commit secrets:** keep passwords, tokens, private keys, and `.env` files out of Git.

## Suggested order
1. Finish `EXERCISES.md` Exercise 1.
2. Practise `fetch` vs `pull`.
3. Open and merge a PR.
4. Practise a conflict using Exercise 3.
5. Explain the workflow aloud as if in a DevOps interview.
