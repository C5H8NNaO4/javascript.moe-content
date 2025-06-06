
# 🧰 Useful Git commands

## 🔗 Init & Update Submodules

> Useful for complex projects using submodules.

```bash
git submodule update --init --recursive
```

- Clones all configured submodules.
- Checks them out to the commit stored in the parent repository.

### ✅ Additional Submodule Commands

```bash
# Add a submodule
git submodule add <repository-url> [<path>]

# Pull new commits in submodules
git submodule update --remote

# Remove a submodule (manual cleanup required)
git submodule deinit -f <path>
rm -rf .git/modules/<path>
git rm -f <path>
```

---

## 👤 Change the Author of Every Commit

> When you've used the wrong Git user/email across all commits.

```bash
git rebase -r --root --exec "git commit --amend --no-edit --reset-author"
```

- Rewrites **entire history** and sets author to current Git config.
- Make sure you've updated your author beforehand:

```bash
git config user.name "Your Name"
git config user.email "you@example.com"
```

> 💡 Consider using [conditional Git config](https://git-scm.com/docs/git-config#_includes) for personal vs. work projects.

---

## 🪢 Enable Symlinks on Windows

> When symlinks appear as plaintext files, enable this config:

```bash
git config --local core.symlinks true
git reset --hard HEAD
```

- Re-enables symlink behavior locally.
- ⚠️ **Warning:** `git reset --hard` will delete untracked files. Make sure the working directory is clean.

---

## 🔄 Reset & Clean Working Directory

```bash
git reset --hard
git clean -fd
```

- Discards all changes (tracked + untracked).
- Use when you want a truly clean slate.

---

## 🔍 Search Commit History

```bash
git log -S'search-term'
```

- Shows commits that added/removed the specified string.

```bash
git log -G'regex-pattern'
```

- Shows commits that match the regex pattern in diffs.

---

## 🧪 Dry Run Push or Clean

```bash
git push --dry-run
git clean -fdn
```

- Shows what would happen **without** actually performing the command.


---

## 🧭 Bisect to Find a Bug

> Use binary search on commit history to find where a bug was introduced.

### 🔹 Basic Workflow

```bash
git bisect start
git bisect bad                # current commit has the bug
git bisect good <commit>     # known good commit (e.g., a tag or old hash)

# Git now checks out commits in-between. Test and mark each as:
git bisect good
# or
git bisect bad

# When finished
git bisect reset
```

- Efficiently narrows down the first bad commit.
- Useful when you don't know which commit introduced the bug.
- Useful when you have a large number of commits between releases.

> 💡 You can automate the process with a script:

```bash
git bisect run ./test-script.sh
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyODIwMTM1MiwtNjcxMTM4NTc1XX0=
-->