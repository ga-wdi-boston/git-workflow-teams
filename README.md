![General Assembly Logo](http://i.imgur.com/ke8USTq.png)

# Team Workflow with Git and GitHub

## Objectives

-   Create branches on a Git repository and make commits on those branches.

-   Combine changes from one branch with another using `git merge`.

-   Combine changes from one branch with another using `git rebase`.

-   Explore different Git workflows that are practiced by teams
     in the real world.

-   In squads, work through our recommended Git workflow
     to build a small project together.

## Prerequisites

-   Basic Git workflow
-   Git Branching, Merging, and Rebasing.

If you're feeling fuzzy on these topics, here's some reading to brush up.

1.  [Atlassian Tutorials: Using Branches](https://www.atlassian.com/git/tutorials/using-branches)

2.  [Atlassian Tutorials: Comparing Workflows](https://www.atlassian.com/git/tutorials/comparing-workflows)

3.  [Atlassian Tutorials: Merging vs Rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
      ('Conceptual Overiew' section only)

## Working With Teams

So far, we've only talked about workflow in the context of working alone.
When you're in the field, working (as you quite probably will) in a team,
 you might find these teams utilizing one of many different kinds of workflows
 in their collective use of Git and GitHub.
Broadly, most of these workflows fall into two buckets:
 **single-remote workflows** and **multiple-remote workflows**.

### Single-Remote Workflows

What defines each of the following workflows is that
 there is a single shared remote repository used by the entire team.
As a result, all collaborators must constantly stay on top of changes
 to a single shared repository.
This is usually accomplished by running `git fetch`,
 which pulls updates from origin, and merging those updates;
 alternatively, you could use `git pull` to do both at once.

#### Centralized Workflow

The remote repo has one single branch on it, `master`.
All collaborators have separate clones of this repo.
They can each work independently on separate things.
However, before they push,
 they need to run `git fetch`/`git pull` (with the `--rebase` flag)
 to make sure that their master branch isn't out of date.

##### Advantages of Centralized Workflow

Very simple - easy to remember how it works.

##### Disadvantages of Centralized Workflow

A high amount of trust is required - if someone forgets to pull,
 they can cause a merge conflict when they push, or worse:
 push code that isn't conflicting but causes other peoples' code to break.
Additionally, because commits may be pushed in any order,
 commits relating to different features may be interleaved.

#### Feature Branch Workflow

This workflow is very similar to the 'Centralized' workflow.
The biggest difference is that there are branches
 (which helps to keep commits related to a new feature isolated),
 and that instead of pushing changes up directly, collaborators
 (a) push up changes to a new remote branch rather than master, and
 (b) submit a pull request to ask for them to be added to
 the remote repo's `master` branch.

##### Advantages of Feature Branch Workflow

Better isolation than Centralized model, but sharing is still easy.
Very flexible.

##### Disadvantages of Feature Branch Workflow

Sometimes it's too flexible --
 it doesn't distinguish in any meaningful way between different branches,
 and that lack of structure can be problematic for larger projects.

#### 'Gitflow' Workflow

Similar to the Feature Branch workflows, but with more rigidly-defined branches.
For example:

-   Historical Branches : `master` stores official releases (see below),
     while `development` serves as a living 'integration branch'
     that ties together all the standalone features.

-   Release Branches : 'release' branches might spring off of `master`,
     pointing to the finished project
     as it existed at a particular moment in time.

-   Feature Branches : pretty much the same as in the prior model.

-   Maintenance/'Hotfix' Branches : branches used
     to quickly patch issues with production code.

##### Advantages of 'Gitflow' Workflow

Highly structured - works well for large projects.

##### Disadvantages of 'Gitflow' Workflow

Sometimes overkill for something small.

### Distributed Workflows

These approaches all use multiple remote repos;
 typically, everyone has their own fork of the 'original' project
 (the version of the repo that's publicly visible
 and is managed by the project maintainer),
 and changes are submitted via pull request.

#### Integration Manager Workflow

One collaborator plays the role of 'Integration Manager'.
This means that they are responsible for managing the official repository
 and either accepting or rejecting pull requests as they come in.

##### Advantages of Integrations Manager Workflow

One person integrates all changes, so there's consistency.

##### Disadvantages of Integrations Manager Workflow

Could get overwhelming for large projects.

#### Dictator/Lieutenants Workflow

This workflow is very similar to the Integration Manager Workflow.
The biggest difference is that
 rather than submitting all pull requests to a single integration manager,
 PRs are funneled through 'Lieutentants', who all report to the 'Dictator'.
Only the Dictator has write access to the official repo.

(This workflow basically has the opposite tradeoff of the previous one).

### Your Turn :: Team Workflow

Though there are a lot of different potential Git workflows for teams,
 for your third project, we'd like you to take the following approach:

-   "Single remote repo"
     (in your case, one for the back-end and one for the front-end),
     managed by the whole team.

-   Feature branching, with all branches merged into a `development` branch.
     The development branch should be fairly clean,
     mostly consisting of merges from feature branches.
     If there are issues with features integrating cleanly,
     you should create a separate branch off of development
     and perform the integration there -
     when finished, you merge these new branches back into development.

> If this were a professional software project,
> you might also use these branches as `release` branches,
> representing complete and fully-functioning
> "release versions" of a piece of software.
> Once a release branch is complete,
> it might get merged with `master`
> and "released" into the world for people to use.

-   When the `development` branch is functional,
     merge it into `master`.
     Barring extraordinary circumstances,
     these merges should be the only commits on the `master` branch.

> For the back-end repo, `master` will used for deployment to Heroku;
> for the front-end repo, you'll be using the `gh-pages` branch for deployment,
> so merge `master` into `gh-pages` when you're ready to deploy.

-   If you use `rebase`, use it only on your personal clone of the repo,
     and only on code that you personally have written.
     _Never, **ever** rebase published code._

To practice this workflow,
 your team will now follow it to create a simple front-end app
 that (in response to a button-click) uses AJAX
 to retrieve (i.e. GET) data from [this API](http://jsonplaceholder.typicode.com/users)
 and renders the data nicely using Handlebars.
Much like the actual front-end repo you make,
 this repo will need a `gh-pages` branch for deployment.

Make commits regularly, in case you need to undo a mistake!

## Further Reading

-   [Git Branches in a Nutshell](http://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell)
-   [Distributed Git Workflows](http://git-scm.com/book/en/v2/Distributed-Git-Distributed-Workflows)
