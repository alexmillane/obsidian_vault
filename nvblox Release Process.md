How to release the core library to the public repo.

## Get Setup

Create a local copy (a checkout on your computer) of nvblox core. I put this in a separate folder called `nvblox_staging`. Clone it from the private repo, but then add a new remote pointing to github nvblox (public) such that this repo has 2 remotes.

## Release process

- Go to repo with 2 remotes
- Checkout the *public tracking branch* of the *private* repo
	- `git checkout public`
- make a new branch off of it v0.0.5
	- `git checkout -b v0.0.5`
- bring the (squashed) changes into the new branch
	- `git checkout --no-overlay release-2.0.0 -- .` 
- commit the squashed changes
	- `git commit -m "v0.0.5`
- push the branch to private remote
	- `git push origin v0.0.5`
- push the branch to public remote
	- `git push public v0.0.5`
- Make an MR on the web interface.
- Merge this MR.
- The public tracking branch on the private has now diverged. Remake it!
	- `TODO`


## Useful notes
Push to a remote where the branch has a different name
```bash
git push origin 
fatal: The upstream branch of your current branch does not match
the name of your current branch.  To push to the upstream branch
on the remote, use

    git push origin HEAD:public
```
