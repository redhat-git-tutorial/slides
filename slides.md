# Git.

---

# Introductions

- Who am I?

  - https://github.com/larsks
  - http://blog.oddbit.com/

- Who are you?

---

# What is git anyway?

- It's a *version control system*, or *VCS*.

- A version control system is a way of tracking changes to one or more
  files.

- This is an example of a *bad* version control system:

        cp myfile.py myfile.py.20180327

---

# What can do you with git?

- Collaborate with other people on a project.

- Explore different solutions to a problem.

- Roll back your code to some point in the past (infinite undo!).

- Fix bugs in older versions of your project without compromising your
  ability to make new features.

- Figure out which change made things stop working.

- Understand who has contributed to a project.

---

# Vocabulary

- **Repository**

    (n) A collection of files and their change history.

- **Commit**

    (n) A set of changes to your **repository**. "The problem is fixed
    in the most recent **commit**."

    (v) To update your **repository** with a new set of changes.
    "I will **commit** the change as soon as it seems to be working
    properly."

- **Branch**

    (n) A named series of **commits** in your **repository**. "The new
    implementation is in the 'testing' **branch**."

---

# Vocabulary

- **Remote**

    (n) A named remote repository. "Cloning a remote repository will
    usually create a **remote** named *origin*."

- **Pull**

    (v) Receive changes from a remote **repository**. "Make sure you
    *pull* from the remote **repository** before starting work on a
    major new feature."

- **Push**

    (v) Send changes to a remote **repository**. "Please **push** your
    changes before you leave for the weekend."

---

# Vocabulary

- **Clone**

    (v) To make a copy of a remote **repository**.  "You must
    **clone** the repository before you can make changes."

- **Fork**

    (n) A diverging copy of a **repository**.  "This project started
    out as a **fork** of ImageMagick."

    (v) To create a diverging copy of a **repository**.  "Before
    submitting a **pull request**, **fork** this **repository** and 
    **commit** your changes there."

- **Pull request**

    (n) A set of one or more **commits** that can be merged into 
    **branch** of a **repository**. "Alice has submitted a **pull
    request** that fixes the problem."

---

# Vocabulary

These last few vocabulary entries aren't particular to `git`, but
they're important topics that I hope we'll have time to discuss.

- **Continuous Integration** (**CI**)

    (n) The use of technologies such as automated testing to allow
    changes to a project to be published as often as possible.

- **Continuous Delivery** (**CD**)

    (n) An extension of **continuous integration** that includes
    processes to automate your release process.

---

# Installing git

- If you have MacOS: Just run `git`, and then follow the prompts to
  install the developer tools when prompted.

- If you have Red Hat/Fedora/CentOS: Run `yum install git`

- If you have Ubuntu: Run `apt-get update; apt-get install git`

---

# Installing git

If you have Windows:

- Go to <https://git-scm.com/download/win>
- Run the installer

---

# Getting started: telling git who you are

Git needs to know who you are in order to identify you in your commit
messages.  Run the following commands to configure this information:

```sh
git config --global user.name "Your Name Here"
git config --global user.email you@your.email.address
```

---

# Getting started: Using git locally

You're about to start a new project and you want to use version
control from the very beginning because you're just that awesome:

```sh
git init MyProject
```

You have an existing project and you realize how much easier and more
productive your life would be if you were to track your changes with
git:

```sh
cd MyProject
git init
git add .
git commit -m 'Initial import'
```

---

# Getting started: cloning a remote repository

You want to start hacking on somebody else's code:

```sh
git clone https://github.com/redhat-git-tutorial/example.git
```

---

# Making changes: what changed?

You've been hacking on your project for hours.  You'd like an overview
of what you changed.

To see a list of modified files, use `git status`:

```sh
$ git status
On branch master

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   example.py

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	awesome_file.py
```

---

# Making changes: what changed?

To see the actual modifications, use `git diff`:

```sh
$ git diff
diff --git a/example.py b/example.py
index f619699..ce9b6cd 100644
--- a/example.py
+++ b/example.py
@@ -1,5 +1,5 @@
 def main():
-    print('This is a very contrived example.')
+    print('Hello Boston!')
 
 if __name__ == '__main__':
     main()
```

---

# Making changes: saving your changes

You've made some changes to your project. You would like to commit
these changes to your git repository.

1. Tell git what files you want to commit.

   ```sh
   git add example.py awesome_file.py
   ```

1. Commit the changes to the repository.

   ```sh
   git commit
   ```

   This will open an editor in which you can type a commit message.
   Alternately, you can provide one on the command line:

   ```sh
   git commit -m 'Fixing some spelling errors'
   ```

---

# Viewing history

You would like to view the history of a git repository.  Use `git
log`:

```sh
$ git log
commit add09696954b831fbcad4d9f713142699e3da6ec (HEAD -> master, origin/master, origin/HEAD)
Author: Lars Kellogg-Stedman <lars@redhat.com>
Date:   Wed Apr 4 00:04:46 2018 -0400

    wrap output statement in a function call

    moving our code into a function allows one to import this module. See
    https://docs.python.org/3/library/__main__.html for details.

commit 03f371c82be4f34c5bae361e30d3de8f38a5eeaa
Author: Lars Kellogg-Stedman <lars@redhat.com>
Date:   Tue Apr 3 23:04:14 2018 -0400

    initial commit
```

---

# Merge conflicts

Let's say we start with a file `example.py` in a remote repository
that looks like this:

```python
def main():
    print('Hello World.')

if __name__ == '__main__':
    main()
```

Bob and Alice have both cloned this locally and are making changes.

---

# Merge conflicts

Alice changes the file so that it looks like this:

```python
def main():
    print('This is a very contrived example.')

if __name__ == '__main__':
    main()
```

She commits her changes:

```sh
$ git add example.py
$ git commit -m 'This is an awful commit message'
[master add99f4] This is an awful commit message
 1 file changed, 1 insertion(+), 1 deletion(-)
```

---

# Merge conflicts

Alice pushes her changes back to the remote repository:

```sh
$ git push
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 347 bytes | 347.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To upstream
   14b2c5e..ca2ea18  master -> master
```

---

# Merge conflicts

Meanwhile, Bob has changed his file so that it looks like this:

```python
def main():
    print('Hello Boston!')

if __name__ == '__main__':
    main()
```

He commits his changes...

```sh
$ git add example.py
$ git commit -m 'Boston is pretty awesome.'
[master 7ce8691] Boston is pretty awesome.
 1 file changed, 1 insertion(+), 1 deletion(-)
```

...and then tries to push them to the remote repository.

---

# Merge conflicts

Unfortunately, Alice got there first.

```sh
$ git push
To upstream
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'upstream'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

Bob is not surprised! This just means that the remote repository has
more recent changes that it did when Bob last pulled it.  Bob knows
that all he has to do is merge those changes into his local
repository.

---

# Merge conflicts

Bob attempts to merge the remote changes:

```sh
$ git pull
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (3/3), done.
From upstream
   eef204c...ca2ea18 master     -> origin/master
CONFLICT (content): Merge conflict in example.py
Automatic merge failed; fix conflicts and then commit the result.
```

Oh no, a conflict! This means that Bob and Alice have made changes to
overlapping lines in the same file.

---

# Merge conflicts

At this point, `git` has annotated the file to show the conflict:

```python
def main():
<<<<<<< HEAD
    print('Hello Boston!')
=======
    print('This is a very contrived example.')
>>>>>>> ca2ea182084c582d4c870f31b4e894e894337dcb

if __name__ == '__main__':
    main()
```

Bob has a decision to make.

---

# Merge conflicts

Bob decides that since variety is the spice of life, the program
should print *both* messages. He modifies the program to look like:

```python
def main():
    print('Hello Boston!')
    print('This is a very contrived example.')

if __name__ == '__main__':
    main()
```

And then commits the result:

```sh
$ git add example.py
$ git commit
```

This will bring up an editor allowing him to edit the commit message,
and will then commit the changes.

---

# Merge conflicts

Bob can now push the changes back to the remote repository.

```sh
$ git push
Counting objects: 6, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 665 bytes | 665.00 KiB/s, done.
Total 6 (delta 1), reused 0 (delta 0)
To upstream
   ca2ea18..84adb2c  master -> master
```

---

# Merge conflicts

The history of the project now looks like this:

```
commit 84adb2ce8698166855ecd69b24b2716353f0ffa7 (HEAD -> master, origin/master)
Merge: 7ce8691 ca2ea18
Author: Bob <bob@example.com>
Date:   Tue Apr 3 23:06:34 2018 -0400

    Merge branch 'master' of .../upstream

commit 7ce8691d997b2bb0b0d2a96e9093f5d7a952a99b
Author: Bob <bob@example.com>
Date:   Tue Apr 3 23:05:15 2018 -0400

    Boston is pretty awesome.

commit ca2ea182084c582d4c870f31b4e894e894337dcb
Author: Alice <alice@example.com>
Date:   Tue Apr 3 23:04:40 2018 -0400

    This is an awful commit message

commit 14b2c5ec20dae1e22efbf48d9854f861211bf628
Author: Alice <alice@example.com>
Date:   Tue Apr 3 23:04:14 2018 -0400

    initial commit
```

---

# Merge conflicts

The state of the repository for people who like pictures:

![Graph of git history for this example](images/merge-conflict-final.svg)

---

# Managing credentials

```sh
git config credential.https://github.com.username yourusername
git config credential.helper 'cache'
```


---

# Resources

- The Official Git Book: https://git-scm.com/book/
- Command reference: https://git-scm.com/docs
- Interactive git tutorial: https://try.github.io/
