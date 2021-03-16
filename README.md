This is a dumb repo to explore automated ways to check if a branch can be merged **before** attempting a merge.

There are great suggestions in the answers to [this StackOverflow question](https://stackoverflow.com/questions/501407/is-there-a-git-merge-dry-run-option).

So far, the best solution I found is to check if the merge can be done without conflicts using `--no-commit` and `--no-ff`.

```bash
git merge $BRANCH_TO_MERGE --no-commit --no-ff &> /dev/null \
  && echo "Can merge" \
  || echo "Cannot merge because of conflicts" \
  ; git merge --abort
```

You can improve and encapsulate this in a dedicated function (or, why not, a custom Git command), like the one in `merge-test`.

To test this approach:

- Checkout this repo
- `git fetch origin`
- Run `./merge-test origin/branch-that-creates-merge-conflicts`
- Run `./merge-test origin/branch-that-does-not-create-merge-conflicts`
