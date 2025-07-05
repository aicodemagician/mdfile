# Full Development Environment Setup Guide

This document outlines the complete setup for a powerful and organized development environment using Visual Studio Code, configured for Node.js, Deno, and the Jujutsu version control system.

## 1. VS Code Extensions

These extensions provide essential tooling for linting, formatting, productivity, and version control.

### Core TypeScript & Node.js

*   **ESLint** (`dbaeumer.vscode-eslint`): The standard for identifying and fixing problems in JavaScript and TypeScript code.
*   **Prettier - Code formatter** (`esbenp.prettier-vscode`): An opinionated code formatter to ensure consistent style across all projects.
*   **Error Lens** (`usernamehw.errorlens`): Improves error visibility by highlighting lines and printing diagnostics inline.
*   **npm Intellisense** (`christian-kohler.npm-intellisense`): Autocompletes npm module names in import statements.
*   **DotENV** (`mikestead.dotenv`): Provides syntax highlighting for `.env` files.

### Deno

*   **Deno** (`denoland.vscode-deno`): The official all-in-one extension for Deno development. It handles formatting, linting, and TypeScript integration, replacing the need for ESLint and Prettier in Deno projects.

### Version Control (Jujutsu & Git)

*   **GitLens — Git supercharged** (`eamodio.gitlens`): Massively enhances VS Code's built-in Git capabilities. It provides inline blame annotations, powerful history navigation, and deep repository insights that work perfectly with a `jj`-managed Git repository.

### Documentation & Diagrams

*   **Markdown Mermaid** (`bierner.markdown-mermaid`): Adds Mermaid diagram and flowchart rendering support to VS Code's built-in Markdown preview.

---

## 2. Workspace Configuration

The core of this setup is the use of two separate VS Code Workspaces to create isolated environments for Node.js and Deno projects. This prevents tool conflicts.

### Node.js Workspace

This workspace is for all Node.js and general-purpose projects. It enables ESLint and Prettier.

**File:** `nodejs-projects.code-workspace`
```json
{
	"folders": [
		{
			"path": "../projects/dev-notes"
		}
	],
	"settings": {
		"editor.defaultFormatter": "esbenp.prettier-vscode",
		"editor.formatOnSave": true,
		"eslint.enable": true,
		"deno.enable": false
	}
}
```

### Deno Workspace

This workspace is exclusively for Deno projects. It enables the official Deno extension and disables ESLint/Prettier.

**File:** `deno-projects.code-workspace`
```json
{
	"folders": [],
	"settings": {
		"deno.enable": true,
		"deno.lint": true,
		"editor.defaultFormatter": "denoland.vscode-deno",
		"editor.formatOnSave": true
	}
}
```

---

## 3. Version Control with Jujutsu (`jj`)

We use Jujutsu (`jj`) for a modern version control workflow, with Git as the backend for compatibility with GitHub.

### Initializing a New Repository

To start a new project that can be pushed to GitHub, use the `jj git init` command. This creates both a `.jj` and a `.git` directory.

```bash
# Navigate to your new project folder
cd /path/to/project

# Initialize a jj repository with a Git backend
jj git init
```

### Basic Workflow

1.  **Make Changes**: Add or modify files. Your changes are automatically part of the working-copy commit.
2.  **Describe Changes**: Add a commit message to the current set of changes.
    ```bash
    jj describe -m "feat: Add new feature"
    ```
3.  **Create a New Commit**: Finalize the current commit and get a clean working copy for the next set of changes.
    ```bash
    jj new
    ```
4.  **Push to GitHub**:
    *   First, link your local repo to the remote on GitHub:
        ```bash
        git remote add origin https://github.com/your-username/your-repo.git
        ```
    *   Then, push your changes to the `main` branch:
        ```bash
        jj git push --change @ --branch main
        ```

### Updating an Existing Branch/Bookmark

If you have made new commits and want to update your `main` branch to point to them, follow this two-step process:

1.  **Move the bookmark:** Update the `main` bookmark to point to your current commit (`@`).
    ```bash
    jj bookmark set main
    ```
2.  **Push the bookmark:** Push the updated bookmark to GitHub.
    ```bash
    jj git push --bookmark main
    ```

This setup provides a robust, organized, and highly efficient foundation for modern software development.