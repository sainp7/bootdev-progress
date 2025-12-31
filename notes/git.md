# Learn git

## Chapter 1: Repository

- `git init` - creates a new repository
- `git status` - shows the status of the repository
- A file can be in one of three states:
    - **untracked**: Not being tracked by Git
    - **staged**: Marked for inclusion in the next commit
    - **committed**: Saved to the repository's history
- By default, any file created in the repository is untracked we need to add it to the staging area by running
  `git add <path-to-file | pattern>`. Without staging, no files are included in the commit—only the files you explicitly
  git add will be committed
- A commit is a snapshot of the repository at a given point in time. It's a way to save the state of the repository, and
  it's how Git keeps track of changes to the project. Use `git commit -m "your message here"` to create a commit.
- `git commit --amend -m "A: add contents.md"`: Change the last commit message
- The `git log` command shows a history of the commits in a repository.
  You can see:
    - Who made a commit
    - When the commit was made
    - What was changed
- Each commit has a unique identifier called a "commit hash". This is a long string of characters that uniquely
  identifies the commit. e.g `5ba786fcc93e8092831c01e71444b9baa2228a4f`

## Chapter 2: Internals

- While commit hashes are derived from their content changes, there's
  also [some other stuff](https://git-scm.com/book/en/v2/Git-Internals-Git-Objects#_git_commit_objects) that affects the
  end hash.
  For example:
    - The commit message
    - The author's name and email
    - The date and time
    - Parent (previous) commit hashes
- Git uses a cryptographic hash function called [SHA-1](https://en.wikipedia.org/wiki/SHA-1) to generate commit hashes.
  They are often called SHA.
- Git is made up of [objects](https://git-scm.com/book/en/v2/Git-Internals-Git-Objects) that are stored in the
  .git/objects directory. A commit is just a type of object.
- All the data in a Git repository is stored directly in the (hidden) .git directory. That includes all the commits,
  branches, tags, and other objects
- `git cat-file -p <hash>`: Show the contents of an object
- `git cat-file -p <commit-hash>`: Shows the contents of a commit:
    - The tree object
    - The author
    - The committer
    - The commit message
- **tree**: git's way of storing a directory
- **blob**: git's way of storing a file
- If you use `git cat-file -p <tree-hash>` you can see the contents of the tree object, that can be blob(s) or tree(s)
- Similarily you can see the contents of a blob object by running `git cat-file -p <blob-hash>`
- Git stores an entire snapshot of files on a per-commit level. However, it does have some performance optimizations so
  that your .git directory doesn't get too unbearably large.
    - Git compresses and packs files to store them more efficiently.
    - Git deduplicates files that are the same across different commits. If a file doesn't change between commits, Git
      will only store it once.

## Chapter 3: Config

- `git config list --local`: List all local Git configuration settings.
- `git config set --global user.name <user_name>`.
- `git config set --global user.email <email_address>`.
- `git config get <key>`: Get the value of a configuration setting.
- Keys are in the format `<section>.<keyname>.` e.g. `user.name` or `<repo_name>.<any_repo_scope_key>`.
- `git config unset <key>`: Unset a configuration setting.
- Typically, in a key/value store, like a Python dictionary, you aren't allowed to have duplicate keys. Strangely
  enough, Git doesn't care. So you can have duplicate keys in your configuration.
- `git config unset --all example.key`: Unset all keys with the key name `example.key`.
- `git config remove-section <section>`: Remove a configuration section.
- There are several locations where Git can be configured. From more general to more specific, they are:
    - **system**: `/etc/gitconfig`, a file that configures Git for all users on the system.
    - **global**: `~/.gitconfig`, a file that configures Git for all projects of a user.
    - **local**: `.git/config`, a file that configures Git for a specific project.
    - **worktree**: `.git/config.worktree`, a file that configures Git for part of a project.
- If you set a configuration in a more specific location, it will override the same configuration in a more general
  location. For e.g, if you set user.name in the local configuration, it will override the user.name set in the
  global configuration.

## Chapter 4: Branching

- A branch is just a named pointer to a specific commit. When you create a branch, you are creating a new pointer to a
  specific commit. The commit that the branch points to is called the tip of the branch.
- Because a branch is just a pointer to a commit, they're lightweight and "cheap" resource-wise to create. When you
  create 10 branches, you're not creating 10 copies of your project on your hard drive.
- `git branch`: List all branches in the current repository.
- `git branch -m oldname newname`: Rename a branch.
- `git branch my_new_branch`: Create a new branch, But it doesn't switch to it.
- `git switch -c my_new_branch`: Creates (if it doesn't already exist) and switch to a new branch.
- `git checkout <branch_name>`: Switch to an existing branch.
- `git log --decorate <no|short|full> --oneline --graph`: Shows a graph of the branches and commits.

## Chapter 5: Merge

- `git merge <branch_name>`: Merges the specified branch into the current branch.
- A merge commit is the result of merging two branches.
- `git log --oneline --decorate --graph --parents`: Shows a graph of the branches and commits. The `--parents` flag
  shows the parent commits of each commit. The `--graph` flag shows the branches as lines.
- If a branch has all the commits from another branch, it can do a fast-forward merge. In this case, Git doesn't create
  a merge commit.