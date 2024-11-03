# git-cherry-pick-onto

Imagine you're working on an issue, and you notice an unrelated problem. To get a fix pushed up, you'll need to:
* Create a commit via `git commit -p`
* Stash your current changes via `git stash`
* Switch to a new branch off of `origin/main` via `git switch -c fix origin/main`
* Cherry-pick your fix over via `git cherry-pick -`
* Push up your changes via `git push --set-upstream origin fix`
* Open a pull request via `gh pr create --web`
* Get back to work via `git switch - && git stash pop`

It's not the end of the world, but it's a bit tedious, and if you're working in a large repo, switching branches can be slow.

`git-cherry-pick-onto` helps to make this situation less painful allowing you to quickly cherry-pick commits onto another branch without modifying your current checkout. It does this by creating a temporary [sparse](https://git-scm.com/docs/git-sparse-checkout) [worktree](https://git-scm.com/docs/git-worktree) which allows it to perform the cherry-pick without affecting your current checkout. By itself this isn't super helpful, but it can be used as a building block for higher-level commands.

For example, [`examples/git-cherry-pr`](examples/git-cherry-pr) opens a [`fzf`](https://github.com/junegunn/fzf) selector with commits that are in your branch but not in `origin/main`, cherry-picks the selected commits onto a new branch, and then opens a pull request in GitHub, reducing the process above from eight commands to two (`git commit -p && git cherry-pr -b fix`), while also running significantly faster.

Currently `git-cherry-pick-onto` is in a proof-of-concept state and could use improvements to its logging and error handling (e.g. handling merge conflicts), but it works well enough for me!
