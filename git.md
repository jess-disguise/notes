# Setup

## Set Name & Email
```bash
git config --global user.name "Your Name"

git config --global user.email you@example.com
```

## Set Default Branch Name
```bash
# set globally 
git config --global init.defaultBranch
```



# Branches 

## Rename Branch
```bash
# Newly created 
git branch -m main
```

### Rename remote and local
```
git branch -m <old-name> <new-name>
git fetch origin
git branch -u origin/<new-name> <new-name>
git remote set-head origin -a
```

## View tracked branch info
```bash
git branch -vv
```


# Remote
> [Remote Branches](https://git-scm.com/book/it/v2/Git-Branching-Remote-Branches)


## List all remotes
```bash
git remote -v
```

## Remove remote
```bash
git remote rm <name-of-remote>
```

## Add remote
```bash
git remote add origin git@github.com:<repository/name.git>
```

## Track an existing local branch to remote
```bash
git branch -u origin/<branch-name>
```

# Undo local changes
> [Stackoverflow](https://stackoverflow.com/a/21697484)
> [Reverting changes in Git](https://www.szakmeister.net/blog/2011/oct/12/reverting-changes-git/) (old)

Discard all local changes (regardless of staged or not)
```bash
git reset --hard HEAD
```

Set your working copy to specific commit but don't lose intermediate commits
```bash
git checkout <commit> -- .
```
>  You'll need to commit to record the fact that you wanted to go back


Set your working copy to a specific commit and discard intermediate commits
```bash
git reset --hard <commit>
```




---

# GPG Keys

## Install GNUPG
[Full instructions from github](https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key)

[Download GNUPG](https://www.gnupg.org/download/index.html)

In WSL: 
```bash
# make a folder for it 
mkdir gnupg

# enter the folder
cd gnupg

# copy the download into your folder
cp /mnt/c/Users/jessd/Downloads/gnupg-2.3.6.tar.bz2 .

# extract the files
tar xvfj gnupg-2.3.6.tar.bz2

# If you get an error about bzip2, install it and try again
sudo apt install bzip2

# generate the GPG keys 
gpg --full-generate-key

# follow the instructions as prompted
```

## Add GPG key to github
```bash
# View the keys 
gpg --list-secret-keys --keyid-format=long
```

Copy the long form of the GPG key ID you'd like to use. 
In this example, the GPG key ID is `3AA5C34371567BD2`:
```bash
gpg --list-secret-keys --keyid-format=long

/Users/hubot/.gnupg/secring.gpg
------------------------------------
sec   4096R/3AA5C34371567BD2 2016-03-10 [expires: 2017-03-10]
uid                          Hubot 
ssb   4096R/42B317FD4BA89E7A 2016-03-10
```

Paste the text below, substituting in the GPG key ID you'd like to use. 
In this example, the GPG key ID is `3AA5C34371567BD2`:
```bash
gpg --armor --export 3AA5C34371567BD2
# Prints the GPG key ID, in ASCII armor format
```

Copy your GPG key, beginning with `-----BEGIN PGP PUBLIC KEY BLOCK-----` and ending with `-----END PGP PUBLIC KEY BLOCK-----`

[Add the GPG key to your GitHub account](https://docs.github.com/en/articles/adding-a-new-gpg-key-to-your-github-account)


## [Tell git about you GPG key](https://docs.github.com/en/authentication/managing-commit-signature-verification/telling-git-about-your-signing-key)
```bash
git config --global user.signingkey <YOUR_GPG_KEY_ID>
```

## [Tell git to sign your commits](https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-commits)
```bash
# Tell git to sign all commits 
git config --global commit.gpgsign true

# Create a signed commit
git commit -S -m "your commit message"

# you will be prompted for GPG passphrase
```

# Toubleshooting

## Failed to Sign
> https://stackoverflow.com/questions/39494631/gpg-failed-to-sign-the-data-fatal-failed-to-write-commit-object-git-2-10-0

If signing not working, test it with command:

```bash
echo "test" | gpg --clearsign
```

If the message is:
```bash 
gpg: signing failed: Inappropriate ioctl for device
gpg: [stdin]: clear-sign failed: Inappropriate ioctl for device
```

Then run `export GPG_TTY=$(tty)`

and try the test again

## PHPStorm/IDE failing to authenticate with git/github
If github account is attached but git commands or menu actions still ask for a github login, the remote URL may be HTTPS instead of SSH.

List all remotes
```bash
git remote -v
```

Remove the incorrect URL
```bash
git remote rm <name-of-remote>
```

Add correct remote URL
```bash
git remote add origin git@github.com:<repository/name.git>
```

Track your existing local branch to the correct remote
```bash
git branch -u origin/<branch-name>
```
