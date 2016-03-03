![General Assembly Logo](http://i.imgur.com/ke8USTq.png)

# Team Workflow with Git and GitHub

## Objectives

-   Create branches on a Git repository and make commits on those branches.

-   Combine changes from one branch with another
     using `git merge` or `git rebase`.

-   Combine changes from one branch with another using `git rebase`.

-   Explain what "merge down, rebase up" means

-   In squads, work through our recommended Git workflow
     to build a small project.

## Prerequisites

-   Basic Git workflow
-   Git Branching and Merging

If you're feeling fuzzy on these topics, here's some reading to brush up.

1.  [Atlassian Tutorials: Using Branches](https://www.atlassian.com/git/tutorials/using-branches)

2.  [Atlassian Tutorials: Comparing Workflows](https://www.atlassian.com/git/tutorials/comparing-workflows)

3.  [Atlassian Tutorials: Merging vs Rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
      ('Conceptual Overiew' section only)

## Git, Together

Although up until now we've been using Git only to manage our own projects,
 it was actually designed as a tool for _teams_ to use,
 so that they could collaborate more effectively.
Since you're much more likely to be working on a development team
 than working individually, it's important to know
 how to use Git in a team setting.

Specific Git workflows will vary from team to team,
 but most are built around feature branching, the practice of
 using separate Git branches to isolate different features of an application
 while they're under development.
Though this is useful even in the context of working individually,
 since it allows you to easily switch which part of the application
 you're working on, where this approach really shines is in a group setting.
By splitting up features over multiple different branches,
 team members can work in parallel on different parts of an application
 without stepping on each others' toes.

There are three core mechanics within Git that a feature branching strategy
 depends on.
Two of them, branching and merging, you've already seen.
Today, we'll introduce a third: _rebasing_.

### Git Rebase, in Pictures

Suppose that you have two branches in your project, `master` and `feature`,
  and that the `feature` branch is currently checked out.

![Rebase](images/rebase_01.png)

If you were to check out the master branch and make a new commit,
 the `feature` branch would no longer point to the end of the `master` branch.

![Rebase](images/rebase_02.png)

How could we update our `feature` branch to incorporate the new change?
One option might be to check out the `feature` branch and merge in `master`.
However, this is a little weird - we're essentially creating a duplicate commit.
What's more, the commit on `master` might not be related to `feature`,
 so it may not make sense for it to be on the `feature` branch.

![Rebase](images/rebase_03.png)

Rebase essentially allows us to pluck off an entire branch and move it so that
 it points to a different commit.
All we need to do is check out the `feature` branch (`git checkout feature`)
 and run the command `git rebase master`; now, the root of the `feature` branch
 points to the new end of the `master` branch

![Rebase](images/rebase_04.png)
![Rebase](images/rebase_05.png)

Now, to be honest, that's not quite what happens - in making the move,
Git actually destroys the old commits and replaces them with new commits
 (with new and different SHAs).

 ![Rebase](images/rebase_06.png)

This is one of the things that can make `git rebase` dangerous.
If you had other branches that pointed to the old commits on `feature`,
 the rebasing process will completely mess those branches up.
This is why, as a rule, you never rebase code that's already been shared -
 you run the risk of breaking other peoples' code.

However, as long as you're only rebasing your own code on top of things,
 `git rebase` is perfectly safe, and if `master` happens to change a lot,
 it's a great way of making sure that `feature` stays up to date.

### The GA Group Project Workflow

Though there are a lot of different potential Git workflows for teams,
 for your group project, we will require you to use the following workflow.

#### Setup (Do Once)

1.  Create a GitHub Organization for your repos,
      and add collaborators as a 'team' within the organization.
      Any repos that you create as part of the project will go
      inside this organization.

2.  Create two empty starting repos within the new GitHub organization.
     Clone those repos down to one team member's computer,
     add in any template files that the repo will be using,
     and then push the updated repos back up to GitHub.
     Additionally, create a new branch called `development` on each repo,
     and push those branches up to GitHub as well.

3.  Have each member of the team clone both repos,
     so that they have their own copies of each.

#### Regular Workflow

On a day-to-day basis, your team will follow a feature branching workflow.
Each time you want to create a new feature for your app,
 you'll go through the following stages.

##### Creating a New Feature Branch

1.  Check out your `development` branch (`git checkout development`)

2.  Ensure that `development` is up to date with
     the `development` branch on GitHub
     by running `git pull origin development`.

3.  Create and check out a new feature branch using
     `git checkout -b my-feature-branch`

##### Integrating a Feature

1.  After you're done working on the branch,
     check in with your group and let them know that
     you're ready to integrate your feature.

2.  Because `development` may have been updated
     in the time since the feature branch was created,
     it's important to make sure that the new feature doesn't conflict
     with anything.
     Run `git checkout development` and `git pull --rebase` to make sure
     that your `development` branch incorporates any updates that were made
     on the repo on GitHub.
     Then, run `git checkout my-feature-branch` and `git rebase development`
     to rebase your new feature on top of the (updated) `development` branch.

3.  If any conflicts were introduced in the previous step,
     work through the code **with your group** and resolve each one;
     when you finish, make a commit.

4.  Now that your branch has been rebased, and you're ready to integrate it,
     push your branch up to GitHub with `git push origin my-feature-branch`.
     and then create a pull request (within your GitHub repo)
     from your feature branch to the `development` branch.

5.  As a group, review the pull request, confirm whether or not
     it can be merged in automatically, and decide whether or not
     to approve the pull request.

     If there are merge conflicts preventing an automatic merge,
     a member of your team will need to resolve those conflicts manually
     on their machine, and then push the newly updated `development` branch
     back up to GitHub.

Once `development` has been updated, other members of the group
 will need to rebase their own feature branches on it (as described in Step 2)
 before they push up those feature branches up to GitHub.

##### Deploying a Working App

Work through the following steps as a group.

1.  Have one member of the group check out `development`
     and pull down the latest version from GitHub.

2.  For this version, check and make sure that the application is working.
     If you have tests, run them.

3.  When you're satisfied that the app is ready to deploy,
     check out the `master` branch and run `git merge development`.

4.  Push the finished version of your code up to GitHub
     (`git push origin master`).

5.  Deploy!

    _If this is your back-end repo,_
    _run `heroku create` to set up a new repo on Heroku,_
    _and push to it by running `git push heroku master`._
    _If this is your front-end repo, run `git checkout -b gh-pages`;_
    _then, remove `bundle.js` and `vendor.js` from your .gitignore file,_
    _make a new commit, and push up your `gh-pages` branch to GitHub._

##### GENERAL GUIDELINES

-   **Always** pull before a merge or rebase.

-   **Never** work directly on either `development` or `master`.

-   **Never** share feature branches;
     if you need two people to work on the same feature,
     they should pair program on the same machine.

-   **Never _ever_** rebase code that's been published.
    Remember, 'merge down, rebase up'!

### Lab : Using the GA Group Project Workflow

To practice the workflow we've prescribed for you,
 your team will now follow it to create a simple front-end app
 that (in response to a button-click) uses AJAX
 to GET data from [this API endpoint](http://jsonplaceholder.typicode.com/users),
 and then renders the resultant data nicely in the page using Handlebars.
You may start by downloading the front-end template as a ZIP
 and dropping those files into your repo.
Your feature branches should be `html-css`, `ajax`, `handlebars`,
 and `ui-behavior`.

Make commits regularly, in case you need to undo a mistake!

## Further Reading

-   [Git Branches in a Nutshell](http://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell)
-   [Distributed Git Workflows](http://git-scm.com/book/en/v2/Distributed-Git-Distributed-Workflows)

## [License](LICENSE)

Source code distributed under the MIT license. Text and other assets copyright
General Assembly, Inc., all rights reserved.
