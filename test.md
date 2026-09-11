# In-Class Pair Activities --- Git Branches, Merge Conflicts, and Collaboration

These activities introduce Git branches in two stages:

1.  **Activity 1:** Create and resolve a merge conflict in a controlled
    environment.
2.  **Activity 2:** Collaborate using two computers, a shared GitHub
    repository, remote branches, `push`, and `pull`.

---

# Activity 1 --- Git Branches and Merge Conflict

## Goal

Create branches, intentionally cause one merge conflict, resolve it
manually, and inspect the Git history.

## Pair Roles

Work in pairs:

- **Driver:** types the commands and edits the files.
- **Navigator:** guides the Driver, checks the steps, and explains
  what is happening.

**Switch roles halfway through the activity.**

---

## Part 1 --- Create the GitHub Repository

1.  Go to GitHub.
2.  Create a new repository named:

```text
git-branch-conflict
```

3.  Select **Add a README file**.
4.  Create the repository.
5.  Copy the repository URL.

Clone the repository:

```bash
git clone <repository-url>
```

Enter the repository:

```bash
cd git-branch-conflict
```

Because the repository was cloned from GitHub, the remote named `origin`
is already configured.

Verify it:

```bash
git remote -v
```

---

## Part 2 --- Create the Starting File

Create a file named:

```text
story.txt
```

Add the following content:

```text
It was a sunny morning.
Alex walked to school.
The city was unusually quiet.
```

Save the file, then run:

```bash
git add story.txt
git commit -m "Add initial story"
git push
```

---

## Part 3 --- Create a Feature Branch

Create and switch to a new branch named `feature`:

```bash
git switch -c feature
```

Check which branch you are currently on:

```bash
git branch
```

Open `story.txt` and change:

```text
The city was unusually quiet.
```

to:

```text
The city was full of people.
```

Save the file and commit the change:

```bash
git add story.txt
git commit -m "Change city description on feature"
```

### Switch Roles

The Driver becomes the Navigator, and the Navigator becomes the Driver.

---

## Part 4 --- Make a Conflicting Change on `main`

Switch back to `main`:

```bash
git switch main
```

Open `story.txt`.

Notice that the change from the `feature` branch is no longer visible.

Change the original line:

```text
The city was unusually quiet.
```

to:

```text
The city was completely empty.
```

Save and commit:

```bash
git add story.txt
git commit -m "Change city description on main"
```

---

## Part 5 --- Merge the Branches

Make sure you are on `main`:

```bash
git branch
```

Merge `feature` into the current branch:

```bash
git merge feature
```

Git should report a merge conflict similar to:

```text
CONFLICT (content): Merge conflict in story.txt
Automatic merge failed; fix conflicts and then commit the result.
```

---

## Part 6 --- Inspect the Conflict

Open `story.txt`.

You should see conflict markers similar to:

```text
It was a sunny morning.
Alex walked to school.
<<<<<<< HEAD
The city was completely empty.
=======
The city was full of people.
>>>>>>> feature
```

Discuss with your partner:

- Which version came from `main`?
- Which version came from `feature`?
- Why couldn't Git automatically choose one?
- What should the final sentence be?

---

## Part 7 --- Resolve the Conflict

Decide what content to keep.

For example:

```text
It was a sunny morning.
Alex walked to school.
The city was full of people.
```

Remove **all** conflict markers:

```text
<<<<<<< HEAD
=======
>>>>>>> feature
```

Save the file.

Mark the conflict as resolved:

```bash
git add story.txt
```

Commit the merge:

```bash
git commit -m "Resolve merge conflict"
```

Push the final result to GitHub:

```bash
git push
```

---

## Part 8 --- Inspect the Git History

Run:

```bash
git log --oneline --graph --all
```

Look at the graph and identify:

- the initial commit
- the `feature` branch commit
- the `main` branch commit
- the merge commit

### Discussion

With your partner, explain:

> Why did the two branches create a conflict?

---

# Activity 2 --- Git Collaboration With Two Developers

## Goal

Collaborate on the same remote GitHub repository from **two different
computers**.

Each student will create their own branch, make a change, push that
branch to GitHub, and participate in resolving a merge conflict.

## Requirements

- Two students
- Two computers
- Two GitHub accounts
- Git installed on both computers

For this activity:

- **Student A** creates the repository.
- **Student B** joins the repository as a collaborator.
- Both students work from their own local clone.

---

## Part 1 --- Student A Creates the Repository

Student A creates a new GitHub repository named:

```text
git-collaboration-practice
```

Select:

**Add a README file**

Then create the repository.

---

## Part 2 --- Add Student B as a Collaborator

Student A opens the repository settings on GitHub and adds Student B as
a collaborator.

Student B accepts the GitHub invitation before continuing.

---

## Part 3 --- Both Students Clone the Repository

### Student A

```bash
git clone <repository-url>
cd git-collaboration-practice
```

### Student B

On the second computer:

```bash
git clone <repository-url>
cd git-collaboration-practice
```

Both students now have their own **local copy** of the same GitHub
repository.

Verify the remote:

```bash
git remote -v
```

---

## Part 4 --- Student A Creates the Initial File

Student A creates:

```text
story.txt
```

Add:

```text
Our team is building a website.
The background color is white.
The project will launch on Friday.
```

Then:

```bash
git add story.txt
git commit -m "Add initial story"
git push
```

---

## Part 5 --- Student B Synchronizes

Student B does **not** automatically receive Student A's new commit.

Student B runs:

```bash
git pull
```

Open `story.txt` and confirm that Student A's file is now available
locally.

### Discuss

Why did Student B need to run `git pull`?

Remember:

- Student A has a local repository.
- Student B has a different local repository.
- GitHub contains the shared remote repository.
- `git push` sends commits to the remote repository.
- `git pull` retrieves and integrates remote changes.

---

## Part 6 --- Student A Creates a Branch

Student A creates and switches to:

```bash
git switch -c student-a
```

Open `story.txt`.

Change:

```text
The background color is white.
```

to:

```text
The background color is blue.
```

Commit:

```bash
git add story.txt
git commit -m "Change background to blue"
```

Push the new branch to GitHub:

```bash
git push -u origin student-a
```

---

## Part 7 --- Student B Creates a Different Branch

Student B creates and switches to:

```bash
git switch -c student-b
```

Open `story.txt`.

Change the **same original line**:

```text
The background color is white.
```

to:

```text
The background color is green.
```

Commit:

```bash
git add story.txt
git commit -m "Change background to green"
```

Push the branch:

```bash
git push -u origin student-b
```

At this point, GitHub should contain three branches:

```text
main
student-a
student-b
```

---

## Part 8 --- Student A Merges First

Student A switches to `main`:

```bash
git switch main
```

Make sure `main` is up to date:

```bash
git pull
```

Merge Student A's branch:

```bash
git merge student-a
```

Push the updated `main` branch:

```bash
git push
```

The `main` branch now contains:

```text
The background color is blue.
```

---

## Part 9 --- Student B Updates Their Local `main`

Student B switches to `main`:

```bash
git switch main
```

Student B's local `main` does not yet contain Student A's merged change.

Retrieve it:

```bash
git pull
```

Open `story.txt` and verify that it now says:

```text
The background color is blue.
```

---

## Part 10 --- Student B Merges Their Branch

Student B is currently on `main`.

Run:

```bash
git merge student-b
```

Git should report a conflict:

```text
CONFLICT (content): Merge conflict in story.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Open `story.txt`.

You should see something similar to:

```text
Our team is building a website.
<<<<<<< HEAD
The background color is blue.
=======
The background color is green.
>>>>>>> student-b
The project will launch on Friday.
```

---

## Part 11 --- Resolve the Conflict Together

Both students should discuss what the final version should contain.

You may keep one version or combine both.

For example:

```text
Our team is building a website.
The background color is blue and green.
The project will launch on Friday.
```

Remove all conflict markers and save the file.

Then Student B runs:

```bash
git add story.txt
git commit -m "Resolve background color conflict"
git push
```

---

## Part 12 --- Student A Synchronizes

Student A's local repository is now behind the remote repository.

Student A runs:

```bash
git switch main
git pull
```

Both students should now have the same final version of `story.txt`.

---

## Part 13 --- Inspect the History

Both students run:

```bash
git log --oneline --graph --all
```

Identify:

- the initial commit
- Student A's commit
- Student B's commit
- the point where the branches diverged
- the merge history
- the conflict-resolution commit

---

# Final Discussion

Answer these questions with your partner:

1.  What is a Git branch?
2.  Why can two branches contain different versions of the same file?
3.  What does `git switch -c <branch-name>` do?
4.  What does `git merge <branch-name>` do?
5.  Why did Git create a merge conflict?
6.  What is the difference between `git push` and `git pull`?
7.  What is the purpose of `origin`?
8.  Why does each developer need their own local repository?
9.  Why should you pull recent changes before integrating your work?
10. How did `git log --oneline --graph --all` help you understand the
    branch history?

---

## Key Commands Used

```bash
git clone <repository-url>
git remote -v
git branch
git switch <branch-name>
git switch -c <branch-name>
git add <file>
git commit -m "message"
git push
git push -u origin <branch-name>
git pull
git merge <branch-name>
git log --oneline --graph --all
```
