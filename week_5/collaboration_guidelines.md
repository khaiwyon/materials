## Collaboration guidelines - working as a team

- While working with teams, it is generally a bad idea to push your work straight into the `master branch`. They generally will cause a lot of problems and you will run into conflict issues.

- The key is working with branches. Here are the steps:

- The first thing you should do is `git pull origin master` to make sure that your master branch is the latest version from the online repository.

- Next, create a new branch with the command `git checkout -b <your-branch-name>`

- Once you've completed your work. Push your branch using `<git push origin <your-branch-name>`

- Go online and create a pull request, ask your teammate to do a code review of your branch. If both of you are okay with it, you may merge your latest branch into the master branch.

- Once done, `git checkout master` to switch to your master branch and `git pull` to pull the latest.

- Always delete your old branches as housekeeping once the branch is merged to master. You can do it with `git branch -d <your-branch-name>`. You can list your branches using `git branch`

## If you run into merge conflicts

- `git pull` to update your master to the latest from the online repository.

- `git checkout <your-branch-name>` to switch to the branch that is having the merge conflict.

- `git merge master` in your conflicted branch to merge the master to your conflicted branch. You will see `warning messages of conflicts` in your terminal.

- `git status` will tell you in red which files needs to be fixed.

- Proceed to one of the via `atom` or your favourite text editor. You will see things like these

```
>>>>>>>>>>>>>>>>>>>>>>>> HEAD
<<<<<<<<<<<<<<<<<<<<<<<<
```

- You may remove those and fix your code to make sure to trim the parts you don't want and keep the parts you want.

- Once you have resolved all files, `git add .` and commit it before pushing with `git push origin <your-branch-name>`.

- Let your teammate know and do another code review. If it is satisfactory, you may merge it into master and follow the cycle again.
