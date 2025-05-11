

## Git Rebase vs Merge: When and Why to Use Each

### [Git](https://git-scm.com/)

#### What is Git?

Git is a version control system which allows you to keep track of all the changes you make to a codebase / repository. It allows you to effectively collaborate and have multiple people work on the same codebase.

It has a few concepts you should be aware of: _repositories_, _PRs_, _branches_ and _commits_.

Once you learned more about these concepts you can start using git for your own projects.

I won't cover the very basics in this article because there's just so much detail you could get into. There is an [extensive guide on git for beginners](https://stackoverflow.com/questions/315911/git-for-beginners-the-definitive-practical-guide) on stackoverflow if you haven't worked with git yet.

However there are a few important things you need to know even when you're familiar with git. Especially if you're working with multiple people on the same project / feature.

The first thing you should learn about is how to use branches _effectively_. This allows you to split work into features, fixes and releases which makes it easy to work on a seperate feature and keep release cycles once your project has grown in complexity.

#### Branching Model

Normally you are working on _feature_<sup>1</sup> and _hotfix_ branches. Once development of a _feature_ meets its [_DoD_](https://www.scrum.org/resources/what-definition-done) it can be merged into the current _release_<sup>2</sup> branch. It makes sense to have _release_ branches which accumulate features and bug-fixes until the next version requirements are met<sup>3</sup>.

<sub>1 - Features represent an increment in value of a product and are usually self contained. It makes sense to have a seperate branch for each feature that's being developed as it contains all the changes that are needed for an increment in value.</sub>

<sub>2 - When developing a product, you usually you have release cycles. A release is a collection of features that marks a finalized version of a product.</sub>

<sub>3 - This means, all features meet their DoD, all tests run successfully. QA and Design is approved. Linters and compilers don't produce errors and the pipeline is _green_ (completes successfully)</sub>

Once a release branch is ready and deployed to production, it should be finalized (_locked_). You can then merge it into _master_ and tag the commit with the correct version number.

Here is an article by Atlassian which covers [different branching workflows](https://www.atlassian.com/git/tutorials/comparing-workflows) such as [_GitFlow_](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow).

Once you are familiar with branching, features and release cycles you can try collaborating with others on a project.

##### There are a few things worth to know when working with together on the same feature branch.

Some of those are rebasing to upstream and preserving history when merging.

#### What's the difference between git rebase and git merge?

<stackoverflow  url="https://stackoverflow.com/a/16666418/1487756">

Please refer to this [question](https://stackoverflow.com/questions/16666089/whats-the-difference-between-git-merge-and-git-rebase)

</stackoverflow>

#### When should you use git rebase and when git merge?

Both merging and rebasing are sometimes neccessary and it's quite easy to know which ones appropriate.

It depends on the state of your **local** branch and the *direction* of changes you are pulling / pushing.

If someone else made changes to the same branch while you were working on your local branches, then you need to **rebase** the remote branch before **merging** your commits into remote.

Suppose you are working with multiple people on the same feature branch. While you are working on your local feature, someone else _merged_ / pushed his changes onto the remote feature branch. Now if you want to _merge_  **your** changes into the same remote branch, you first need to _pull_ the changes made to the upstream.

This is where you need a _rebase_. This will move your local commits behind / on top of the changes made to the remote feature branch by others and results in a linear history of commits which can cleanly be merged back into the remote feature branch.

This is where another questions pops up. How do you merge a _feature_ branch into the _release_ branch.

You basically have two options: --no-ff and --squash

#### What's the difference between --no-ff and --squash?

These are two important options affecting how your commit history looks which in turn affects how easy it is to revert changes at a later stage.

Generally, simplicity should be preferred and a _linear_ history is simpler than one with complicated merge commits.

When merging a series of commits (a feature) into your release branch, you can either choose whether you want to keep every single commit of the feature branch in the history (--no-ff), or if you want to combine all commits into a single commit with a single commit message (--squash).

The default merge behaviour simply _fast forwards_ your HEAD to the tip of your feature branch which loses the information that a feature has been developed in a seperate branch, but keeps all of the commits with their individual commit message.

The commit messages are important because they can be shown in your IDE next to the line of code which is very helpful when working with others.

--squash on the other hand _squashes_ all commits into a single commit which means that every line in a _blame_ shows the same message.

This is fine in many cases, however you need to be aware that if you want to revert things later you can only revert the whole squashed commit, not parts of it.

The most accurate command you can use in terms of preserving history, commit messages for blames and revertability of single commits is git merge feat/my-feature --no-ff

IMHO this should be standard and you should need to actively choose to fast forward (which is apt in some cases).

---
I think these are some of the most important things to know about git and if you mastered all this, then using git will be a walk in the park.

_Note: There's even more you can do in git like bisecting which is very useful and something every mid to senior developer should know how to use, but that will be covered in another post._
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTc1NjkxNjg1Nl19
-->