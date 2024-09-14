# [shroudage](https://github.com/nxsy/shroudage)

shroudage is a simple shell script wrapper around `age` for use as a git
filter to transparently encrypt/decrypt contents - storing files encrypted
in the repository, but unencrypted in your working copy.

Use of this requires vigilance to ensure these filters are acting as
intended, so check any commits before pushing.

## Using

Run `shroudage init` to set up your repository - whether it is newly-created
or newly-checked out.  On first use on a repository, it will generate a
passphrase-encrypted key, and set up a `.gitattributes` file (if one does
not exist already) to handle files in the `content` directory.  In all
cases, it will decrypt this encrypted key locally, and configure git
filters.

This will copy this script into your repository at `.scripts`, so you do
not need anything external to the repository in future.

If you're checking out a repository, you'll want to run `git restore .`
after init to force decryption of the working copy (which would be the
encrypted contents in the initial checkout).

## Validating

After staging a change, use `git diff --staged --no-textconv` to see what
will be in your commit.  Use `git show --no-textconv` to see what your
commit looks like in the local repository before pushing elsewhere.

## Testing

If you have checked out the shroudage repository, it is using shroudage
itself.  Look at `content/README.md` for the encrypted contents in the
working directory, and then run `./shroudage init` to update the repository
git filters (the key's passphrase is `unsecure`).  After running `git restore
content`, you should now see the unencrypted contents in `content/README.md`:

    This should be encrypted in the repository.
