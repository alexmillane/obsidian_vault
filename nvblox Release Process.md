  

## Process to release the core lib

Create a local copy of nvblox core (private) & github nvblox (public) (2 remotes)
preview branch of private matches pre-release branch of public

```bash
git checkout preview
git checkout --no-overlay main -- .
git commit -am "This is a great new change"
git push
git checkout pre-release
git merge preview
git push
```




## Useful notes
Push to a remote where the branch has a different name
```bash
git push origin 
fatal: The upstream branch of your current branch does not match
the name of your current branch.  To push to the upstream branch
on the remote, use

    git push origin HEAD:public
```
I just did what it says.


