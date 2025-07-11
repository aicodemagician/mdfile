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

## B. The Daily Workflow

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
    *You can see your history at any time with `jj log`.*

---

## C. Syncing with GitHub

Instead of pushing branches directly, the `jj` workflow uses bookmarks (which are like lightweight, local-first branches).

1.  **Set or update your bookmark:** To mark your current commit as the one you want to push, update the `main` bookmark.
    ```bash
    # This moves the 'main' bookmark to your current commit (@)
    jj bookmark set main
    ```
2.  **Push the bookmark:**
    ```bash
    jj git push --bookmark main
    ```
This command tells `jj` to update the `main` branch on the `origin` remote to match the commit pointed to by your local `main` bookmark. This is the only push command you need for regular updates.