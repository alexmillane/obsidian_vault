
## Push
To push a new CL
```bash
git push gerrit HEAD:refs/for/master
```

> Actually apparently I should just use git review from the first commit


## Change commit messages
```bash
git rebase --interactive
```
Then replace pick with `r`
This redoes commits past master.

## If you fuck up and amend an outdated commit

Two branches:
* `accidental_hash`
* `head_hash`
Goals to get back
* Checkout head: `git checkout head_hash`
* Overlay changes in accidental hash: `git checkout --no-overlay accidental_hash -- .`
* Commit amend
