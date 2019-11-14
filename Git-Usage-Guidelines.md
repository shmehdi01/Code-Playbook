## Beginners 

Please read following books/blogs
* [Git for teams](http://pepa.holla.cz/wp-content/uploads/2016/01/Git-for-Teams.pdf)
* [Pro Git](https://git-scm.com/book/en/v2)

Apart from this, we suggest reading whatever material interests you. 

- [ ] Yes, I have read these books.

Please move further down in this document only if you know following things 

- [ ] How to make a repository. 
- [ ] Cloning a repository. 
- [ ] What is branch and how to create it.
- [ ] `git remote add origin <url>`
- [ ] `git push origin <branch>`
- [ ] `git pull origin <branch>`
- [ ] `git merge <branch>`
- [ ] `git checkout -b <branch>`
- [ ] How to create a pull request 
- [ ] How to review code 
- [ ] How to merge pull request 
- [ ] How to see logs
- [ ] How to revert commits
- [ ] `git reset --hard HEAD`

## Install [git-extras](https://github.com/tj/git-extras/blob/master/Installation.md) 

Please install git-extras and read its [commands](https://github.com/tj/git-extras/blob/master/Commands.md). Please be familiar with those.

Please move further down in this document only if you have understood these commands and tried on your machine. 

- [ ] Yes, I understand these commands and I have tried on my machine.
 
## Branches

### How to create branches

* Always create a new branch from `dev` for every new feature you are working on. Please make sure you have pulled latest dev before creating a branch.
* Bug fixes should always be in new branches and those branches should be created from dev. 
* One branch should do just one thing. Please do not mix many features in one branch. 
* Never create nested branches. ALWAYS create a new branch from `dev`. If you need to use a code in other branches please use cherry-pick instead of making a nested branch. 
* Never mix code refactoring and feature in one branch. Please create a separate branch for code refactoring.
* When working on a feature, Please try to finish that feature in the branch you have created. That branch should contain production ready code always.
* Code commits in a branch should always be related to what the name of the branch is. For example, never mix two features in a branch and name it based on one feature. 

### How to name branches

We use `feature/`, `bug/` and `refactor/` as a prefix in the name to suggest what is the purpose of a branch. After the prefix comes the name of the branch which reveals its intent. For example here are some use cases. 

```
feature/new-feature
bug/make-it-work
refactor/make-it-awesome
```

## How to write good commit message
 - [ ] [How to write a git commit message](https://chris.beams.io/posts/git-commit/)
 - [ ] [GitCommitMessage](https://wiki.openstack.org/wiki/GitCommitMessages)

## Commit message format 

* Every commit should start with keywords like "Refactor:, Bug:, Feature:, Chore:", followed by name feature/bug you are working on. Something like that "Bug: home: Fix crash on home button click"
* Commits should be small and should only do what its commit message says. For example, If it says that it fixes crash on home button click, it should only fix crash on Home button click and nothing more. Not even a small change needs to be done here. 
* Never mix refactor commit with something else. And refractor commit format should be like this "Refactor: removed redundant function from HomeActivity"
* Please always try to create a clean commit history and do not add nonsense commits. 
* Commits should be done in such a way that they can be reverted quickly when a need comes in future. 
* When the project is in starting phase and you are writing code/files from scratch, do not make small-small commits. Make sure commits are big and contain that one feature you are working on.

### How to create a pull request

* Please give Title to pull request. 
* Please provide a description in pull request about what a pull request does. Pull request descriptions should be concise and well written. The reviewer should be able to copy this description straight into the release notes, instead of figuring out what changed or was fixed.
Besides, a description with more information could be helpful, for example:

- If it's a static UI, a screenshot would do. Can be ignored. 
- If it's an interaction, a gif would help a lot. A good tool to make gifs is [Licecap](http://www.cockos.com/licecap/). Can be ignored. 
- If it's a bug, steps to reproduce are very useful to help understanding context. Can be ignored. 

* If your project is in the initial state and you are pushing the code first time for a particular, Please squash your commits into one. Therefore you are going to push one commit for the whole feature for the very first time. 
* Your pull request should be of the reasonable length so that code reviewer can review it comfortably. 
* Please make sure there are no conflicts in the pull request. 
* If you have created a pull request which is work in progress and you do not want to merge it, Please add `[WIP]` as prefix to the pull request title. 
* Please make sure you have reviewed the code first and all the guidelines are followed. 
* Please make sure that you have run the code yourself and tested the UI/Functionality yourself. 
* After created a pull request please leave a commit for reviewer. For example, `@aashishdhawan please review.`
* Author/Developer will be held responsible if his pull request is not merged within 2 days. Please ask code reviewer to finish reviewing it. 
* There should not be any open pull request which is not needed or which is obsolete.

#### My pull request depends on another pending pull request. What to do?

If your next pull request is really that intimately related to your last, consider continuing working on it instead of opening another.

If it really makes sense to open another, see if you can use cherry-pick, if not,  it's okay to base your next pull request on your unmerged branch, just make sure to make it clear in the PR description.
Finally, make sure to merge any changes you've made to its parent during its review.

Before merging a pull request, your code has to be reviewed, so that we can learn from you, 
point out your silly mistakes, and/or post a sufficient amount of gifs. The reviewing
process is important. It is better for you to have another person backing you up. More eyes
can mean fewer bugs and more consistency throughout.


#### Reviewing pull requests

As a reviewer, you should ideally be a core team member and have enough context
to make a thorough review of the committed code, just by reading it. However,
there are times when the pull request is a bit on the large side, or it is
referencing existing code not found in the PR, or you for some other reason do
not have a good enough context to review anything else than code style and typos.
In these cases, we encourage you to physically (or virtually) approach the PR
submitter and go through the pull request together.

Be warned, though: By doing this, you will both need to explain your thoughts and
discuss other alternatives. Of course, this means you are putting yourself at risk
of sharing your knowledge and/or learning some new stuff, so please be careful not
to end up being even more awesome than you already are.

As a reviewer your job is to be the extra pair of eyes, so the code will get twice 
as good. You'll look at things such as code style and structure. You'll discuss things 
that you might've done differently, point out possible more elegant solutions based 
on something you've learned, and so on. The goal here is to make the whole team better 
in the process.

Keep in mind that you are reviewing something that someone else has worked on really 
hard, so always be respectful. These guidelines for giving feedback are a 
good reference:

- Familiarise yourself with the context of the issue.
- If you disagree strongly, consider giving it a few minutes before responding; think before you react. Try to empathise.
- Ask, don’t tell. (“What do you think about trying…?” rather than “Don’t do…”)
- Explain your reasons why code should be changed. (Not in line with the style guide? A personal preference?)
- Offer ways to simplify or improve code.
- Avoid using derogatory terms, like “stupid”, when referring to the work someone has produced. Don’t shame, no one started out knowing.
- Be humble. (“I’m not sure, let’s try…”)
- Never deal in absolutes. Unless needed. (“We should never store sensitive data in plain text.”)
- Aim to develop professional skills, group knowledge and product quality, through group critique. That goes both way: try to be open and welcoming to criticism.
- Be aware of negative bias with online communication. (If the content is neutral, we assume the tone is negative.) Try to use positive language instead.
- Use emoji to clarify tone. Compare “ok” to “ok 😃.”

[Based on GitHub's "How to write the perfect pull request".](https://github.com/blog/1943-how-to-write-the-perfect-pull-request)

`NEVER merge a pull request if you think code is not production ready.`

If everything is fine, end code review and leave a comment `@developer please merge when ready.` The developer will himself/herself merge it. 

After you have merged, you should delete the branch and smile. The smile is critical 
part of the process, so don't forget this.

#### Code Linting

We try to keep our whitespace and code style consistent, one of the tools we use to achieve this is [swiftformat](https://github.com/nicklockwood/SwiftFormat). We recommend installing it using [homebrew](https://brew.sh), for easy running and updating. For most projects you can run it from the root folder, but for projects using Cocoapods/Carthage, it’s recommended to run it from the top subfolder (usually has the same name as the project), to avoid reformatting third-party code.

Android developers and other projects should use linting almost always. 


[_This is partly based on Hyper's Git and GitHub guide._](https://github.com/hyperoslo/playbook/blob/master/GIT_AND_GITHUB.md)
  

### Code Quality responsibility 
Code Quality responsibility lies on both, the original author and the reviewer of the pull request. 

The original author needs to verify few things before assigning a pull request to a reviewer. 

 - [ ] That he has himself checked the pull request once and has made sure everything is OK and the diff is exactly what he wanted to push. 
 - [ ] That he/she has verified that there are no spelling mistakes and naming conventions are correct.
 - [ ] That he/she has used the commit format as stated above. 
 - [ ] That he/she is following the Code Style Guide as mentioned in project workflow book and that he has verified there his code standard is correct. 
 - [ ] That he/she has attached gif/pictures in pull request if this helps reviewer. This is especially useful when UI work has been done. 
 - [ ] That he/she has assigned a reviewer to a pull request. 

The reviewer needs to take care of following things 

 - [ ] Since a code reviewer is the last line of defense, he/she will be held responsible for poor implementations and code quality. 
 - [ ] He/she has to make sure the original author has taken care of above-said things (his responsibilities.)
 - [ ] Reviewer has right to reject pull request as long as he/she is not satisfied. 
- [ ] Please reject pull request at once if you feel developer is doing same mistakes again and again, even after you pointed that out. 


## Responsibilities if bugs/incomplete features are pushed into production 

* Level 1: A original author is the first one to be held responsible for the code he/she has written. Therefore the developer must always test the code he/she is pushing. The developer should test it himself/herself and check for various use cases. 
* Level 2: Code reviewer is the second line of defense. He/she should check the pull request properly. Code reviewer will be held responsible if `Level 1` pushed the bugs and went un-noticed at `Level 2`.
* Level 3: Team lead is the third line of defense. Most of the case Code reviewer and Team Lead will be the same person but if that is not the case `Level 3` will be held responsible if Level 2 fails. 
* Level 4: QA, A bug should never get pass through here. Most important part. If Level 1,2,3 do their job properly, QA will find bugs which are rare. 
* Level 5: Project Manager: A bug should not get pass thorugh here. Its PM's responsibilty if clients are being delivered buggy builds.

If client reports bugs, each one of the above will be asked to explain how they failed in thier duties.

## Release Process

Steps for releasing a new version of your app:

1. Switch to the `master` branch
2. Bump the project's version number (make sure to use [semantic versioning](http://semver.org/))
3. Create and upload the archive to HockeyApp
4. Commit and push your changes to the `master` branch
5. Create a [release on GitHub](https://help.github.com/articles/creating-releases/). If the release uses the `staging` server, mark it as `pre-release`

* [How to avoid merge commits in commit history](http://kernowsoul.com/blog/2012/06/20/4-ways-to-avoid-merge-commits-in-git/)
