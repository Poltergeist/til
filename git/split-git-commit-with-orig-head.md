# How to split a Git commit and preserve message using ORIG_HEAD
A colleague told me he'd accidentally committed something. Now he wanted to remove it from the previous
commit and then put in a new commit. This is called splitting a commit, and here's the best way I know of how to do it.

## Solution
1. `git reset --soft HEAD~` Undo the last commit, but preserve "Changes to be committed"
1. `git reset <some/path>` Exclude what you want
1. `git commit -C ORIG_HEAD` Commit with the same message as before, which was stored in `ORIG_HEAD`

## Example
```bash
❯❯❯ echo hej > hej.txt

❯❯❯ echo hej2 > hej2.txt

❯❯❯ git add hej.txt hej2.txt

❯❯❯ git commit -m 'Add hej.txt'
[master f919e9d] Add hej.txt
 2 files changed, 2 insertions(+)
 create mode 100644 hej.txt
 create mode 100644 hej2.txt

❯❯❯ echo "Oops, I accidentally added hej2.txt as well"
Oops, I accidentally added hej2.txt as well

❯❯❯ git reset --soft HEAD~

❯❯❯ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   hej.txt
	new file:   hej2.txt

❯❯❯ git reset hej2.txt

❯❯❯ git commit -C ORIG_HEAD
[master b880e4d] Add hej.txt
 Date: Fri Jan 19 11:22:31 2018 +0100
 1 file changed, 1 insertion(+)
 create mode 100644 hej.txt

❯❯❯ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	hej2.txt

nothing added to commit but untracked files present (use "git add" to track)
```

## Background
I've done this many times before, but with different approaches that all were non optimal.
One of my previous crazy solutions was to undo it manually and then make a fixup-commit,
after that revert the fixup commit without committing and then use autosquash.
`git commit --fixup HEAD`, `git revert HEAD`, `git rebase -i --autosquash HEAD~3`
...not as good as the solution above with `ORIG_HEAD`! :)

Related reading:
- `man git-reset` The git reset manual
- https://stackoverflow.com/a/967611/2037928 HEAD and ORIG_HEAD in Git
- https://emmanuelbernard.com/blog/2014/04/14/split-a-commit-in-two-with-git/#comment-3274061909

## Crazy aliases
If you like git aliases, here's one I just (re)invented

`~/.gitconfig`
```gitconfig
[alias]
split-head = "!f() { git reset --soft @~ && git reset $@ && git commit -C ORIG_HEAD; }; f"
```

```bash
❯❯❯ git split-head hej2.txt
[master b9ccd93] Add hej.txt
 Date: Fri Jan 19 12:45:40 2018 +0100
 1 file changed, 1 insertion(+)
 create mode 100644 hej.txt

❯❯❯ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	hej2.txt
```

```bash
❯❯❯ git split-head -p
diff --git a/hej.txt b/hej.txt
new file mode 100644
index 0000000..1b5b67f
--- /dev/null
+++ b/hej.txt
@@ -0,0 +1 @@
+hej
Unstage this hunk [y,n,q,a,d,/,e,?]? n

diff --git a/hej2.txt b/hej2.txt
new file mode 100644
index 0000000..fb793ab
--- /dev/null
+++ b/hej2.txt
@@ -0,0 +1 @@
+hej2
Unstage this hunk [y,n,q,a,d,/,e,?]? y

[master 2486910] Add hej.txt
 Date: Fri Jan 19 12:45:40 2018 +0100
 1 file changed, 1 insertion(+)
 create mode 100644 hej.txt
```
