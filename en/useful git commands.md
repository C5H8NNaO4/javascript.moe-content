## [git] init submodules

_This is a command you will need when you work on complex projects with nested submodule. I usually don't bother typing all that and just copy it from somewhere_

`git submodule update --init --recursive`

This command clones all conigured submodules and checks them out to the commit stored in the parent repository.

## [git] Change the author of every commit

_Suppose you accidentally used a wrong git user for all of your commits in a project. This can e.g. happen when you work on a private project on a company pc. When you publish the code on your private github account it will have your companies email. In order to reset, you can execute this command_

`git rebase -r --root --exec "git commit --amend --no-edit --reset-author"`

This will change the  **entire**  commit history and resets the author to the currently configured value. So make sure you change your author or create a seperate git config for private projects.

## [git] Enable symlinks

*Suppose you have a cross platform project that relies on symlinks, but you forgot to check the box when running the git installer.

_When you're on Windows and  **symlinks show up as plaintextfiles containing a path**, then symlinks are disabled in the git config. You can enable them using these two commands_

```bash
git config --local core.symlinks true
git reset --hard HEAD
```

_**Warning:**  This will wipe any untracked files, so make sure the working directory is clean._
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY3MTEzODU3NV19
-->