# git-branching-test
Experimenting with git branches

When I get a merge conflict:

 new branch! I expected a merge conflict! Yay! #1 
 
 https://github.com/siliconninja/git-branching-test/pull/1

What i did on my local repo with the newest add-new-feature changes but not the master changes:
````
$ --> git fetch origin
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://github.com/siliconninja/git-branching-test
   af65022..5ab2d3c  master     -> origin/master
$ --> git pull
Updating af65022..5ab2d3c
Fast-forward
 test.html | 4 ++--
 1 file changed, 2 insertions(+), 2 deletions(-)
$ --> git merge master
Auto-merging test.html
CONFLICT (content): Merge conflict in test.html
Automatic merge failed; fix conflicts and then commit the result.
````

https://stackoverflow.com/questions/11091969/conflict-content-merge-conflict-in
Resolve a conflict by editing the file to **manually merge the parts of the file that git had trouble merging**. This may mean **discarding either your changes or someone else's or doing a mix of the two**. **You will also need to delete the <<<<<<<, =======, and >>>>>>> in the file.**

Just like SVN

````
$ --> nano test.html 
<!doctype HTML>
<html>
<<<<<<< HEAD
This is test webpage
=======
This is a test page
>>>>>>> master
</html>
````
Changed to:
````
<!doctype HTML>
<html>
<!-- I compromised with my &quot;project partner&quot;, he wants the following text: -->
This is a test page/webpage
</html>
````
Now:
````
$ --> git merge master
error: Merging is not possible because you have unmerged files.
hint: Fix them up in the work tree, and then use 'git add/rm <file>'
hint: as appropriate to mark resolution and make a commit.
fatal: Exiting because of an unresolved conflict.
````
Ok, still have to `git add (changed files)`
`$ --> git add .`
````
$ --> git commit -m "I compromised with my imaginary partner and changed the file"
[add-new-feature 3a58665] I compromised with my imaginary partner and changed the file
````
Since i worked on the add-new-feature branch, I still have to use the (origin is the github source repository) `git push origin add-new-feature` instead of just `git push`, that assumes `git push origin master` (the **master** branch is being pushed to, not the one i've currently **checked out** (using `git branch`).)

So when I do
````
$ --> git push
fatal: The current branch add-new-feature has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin add-new-feature
````
I can do either that or (i do want to push to Github upstream)
````
$ --> git push origin add-new-feature
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 8 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 432 bytes | 432.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/siliconninja/git-branching-test
   9e7ccad..3a58665  add-new-feature -> add-new-feature
````
Whew that fixed it

(to see im on the add-new-feature branch)
````
$ --> git branch
* add-new-feature
  master
````

Step 1: From your project repository, **bring in the changes** and test.

git fetch origin
git checkout -b add-new-feature origin/add-new-feature
**git merge master** (gets the new changes)

Step 2: Merge the changes and update on GitHub.

git checkout master
git merge --no-ff add-new-feature
git push origin master

Now I can do a simple `git checkout master` and `git merge --no-ff add-new-feature` (to keep commit messages, https://thenewstack.io/dont-mess-with-the-master-working-with-branches-in-git-and-github/)

Step 5: Merge working branch changes to the master.

In this case, since we want to merge from our working branch, where the “hello_octo_world” file exists, to our master branch, we need to be on the master.

> Once on the master branch, all we have to do is run the merge command. The best way to do this is to type “git merge –no-ff” — the additional “–no-ff” tells git we want to retain all of the commit messages prior to the merge. This will make tracking changes easier in the future:

For some reason the commit thingy was weird and I had to exit.
Re-committing:
````
$ --> git commit -m "merge imaginary partner's changes"
[master 08a6e05] merge imaginary partner's changes
````
And then push the final changes (`git push origin master`). Merge complete!

And github figures that out very quickly!
````
Pull request successfully merged and closed

You’re all set—the add-new-feature branch can be safely deleted.
````
