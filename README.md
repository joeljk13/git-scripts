git scripts
===========

A bunch of scripts designed for both interactive and script use. They assume
they're in ~/.git-scripts, so ~/.git-scripts should be in your PATH.

git-are-no-conflicts
--------------------

    git are-no-conflicts <branch1> <branch2> [<branch3> ...]

Exits with a exit code of 0 if the given branches can be merged without
conflicts; otherwise, exists with an exit code of 1.

git-branches
------------

Lists all local branches in the repository. Designed for scripting, so you can
do something like

    for branch in $(git branches); do
        # Do something with $branch
    done

git-current-branch
------------------

Gets the current branch, or exists with a exit code of 1 if no branch is
checked out. This was also designed for scripting, although git-ref is probably
more useful.

git-is-repo
-----------

A quick and simple way to tell if the current working directory is in a git
working tree. Exits with a code of 0 if it is in a working tree, 1 otherwise.

git-ref
-------

    git ref [--short]

A useful scripting tool for figuring out where HEAD is - it prints a
symbolic ref name if it can, and the SHA1 hash if it can't. This too is
designed for scripting, so you can do this:

    ref=$(git ref)
    # ...
    # Do whatever checkouts you want
    git checkout branch
    # ...
    # Go back to where you were, so the user isn't
    # surprised that HEAD changed on them
    git checkout "$ref"

This command also can be useful for shell prompts.

git-update-all
--------------

This command does a `git fetch --all`, and then it checks out each branch and
tries to rebase it against the remote branch its set to track (if such a branch
exists). This command relies of `git-ref` to get back to where it was, and
`git-are-no-conflicts` to make sure avoid trying to automatically update
branches where they will be conflicts.
