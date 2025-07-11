# A Guide to Version Control with Jujutsu (`jj`)

This document outlines a modern version control workflow using Jujutsu (`jj`), with Git as the backend for compatibility with services like GitHub. This approach provides a more intuitive and powerful local development experience.

---

## A. One-Time Setup for a New Project

Follow these steps when starting a new project you want to host on GitHub.

1.  **Create a new repository on GitHub.** Do not initialize it with a README or license file. Just create a blank repository.
2.  **Initialize the local repository:** In your project folder, run:
    ```bash
    # This creates both a .jj and a .git directory
    jj git init
    ```
3.  **Add the GitHub remote:**
    ```bash
    # Replace with your own GitHub URL
    git remote add origin https://github.com/your-username/your-repo.git
    ```
4.  **Make your first commit:** Add some files, then describe the change.

    ```bash
    # Example: create a README
    echo "# My New Project" > README.md

    # Describe the working-copy commit
    jj describe -m "Initial commit"
    ```

5.  **Push to GitHub for the first time:**
    ```bash
    # Push the current commit (@) to the 'main' branch on the remote
    jj git push --change @ --branch main
    ```

---

## B. Working with `.gitignore`

Jujutsu automatically respects the `.gitignore` file from the underlying Git repository. This file tells version control which files and directories to ignore, such as build artifacts, log files, or editor-specific settings.

### How it Works

1.  **Create or edit the `.gitignore` file** in your project's root directory.

2.  **Add patterns** for the files you want to ignore. For example:

    ```
    # VS Code specific files
    .vscode/
    *.code-workspace

    # Log files
    *.log
    ```

3.  **Commit the `.gitignore` file itself.** For the ignore rules to apply, the `.gitignore` file must be part of your project's history. When you create or change it, `jj` will detect it as a file to be committed.

    ```bash
    # Check the status, you will see the .gitignore file
    $ jj status
    Working copy changes:
    A .gitignore

    # Describe the change to commit it
    jj describe -m "feat: Add .gitignore to exclude logs and VS Code files"
    ```

4.  **New.** Finalize the commit and create a new, empty working-copy commit to start the next task.
    ```bash
    jj new
    ```
    _You can see your history at any time with `jj log`._

Once committed, any new files matching the patterns in `.gitignore` will be ignored by `jj` and will not appear in `jj status`.

---

## C. The Daily Workflow

The local `jj` workflow is a simple, repeating cycle.

1.  **Code.** Make changes to your files. Your changes are automatically included in the "working-copy commit."
2.  **Describe.** When you reach a logical stopping point, describe what you did.
    ```bash
    jj describe -m "feat: Implement user authentication"
    ```
3.  **New.** Finalize the commit and create a new, empty working-copy commit to start the next task.
    ```bash
    jj new
    ```
    _You can see your history at any time with `jj log`._

---

## D. Syncing with GitHub

Instead of pushing branches directly, the `jj` workflow uses bookmarks (which are like lightweight, local-first branches).

1.  **Set or update your bookmark:** To mark your current commit as the one you want to push, update the `main` bookmark.

    ```bash
    # This moves the 'main' bookmark to your current commit (@)
    jj bookmark set main
    ```

    > **Note:** If you have rebased or amended commits that were already bookmarked, you may see an error like `Refusing to move bookmark backwards or sideways`. This is a safety feature. If you are sure you want to move the bookmark, use the `--allow-backwards` flag:
    >
    > ```bash
    > jj bookmark set --allow-backwards main
    > ```

2.  **Push the bookmark:**
    `bash
jj git push --bookmark main
`
    This command tells `jj` to update the `main` branch on the `origin` remote to match the commit pointed to by your local `main` bookmark. This is the only push command you need for regular updates.
