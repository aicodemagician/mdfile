# A Guide to a Modern Development Environment

This document provides a complete, step-by-step guide to setting up a powerful and organized development environment. It uses Visual Studio Code with a dual-workspace system for Node.js and Deno projects, and features a modern version control workflow with Jujutsu (`jj`).

---

## Core Philosophy: Isolate to Innovate

The foundation of this setup is the use of two separate VS Code Workspaces: one for Node.js and one for Deno.

**Why?** To prevent tool conflicts. The Deno ecosystem has its own built-in linter and formatter, while the Node.js ecosystem relies on separate tools like ESLint and Prettier. By keeping them in separate workspaces, we ensure the correct tools are active for each project type, eliminating errors and configuration headaches.

For a deeper dive into their differences, see the Deno vs. Node.js - A Quick Guide.md.

---

## Step 1: Install Core Tools

Before configuring VS Code, ensure the core command-line tools are installed on your system.

*   **Node.js:** Required for the Node.js workspace.
*   **Deno:** Required for the Deno workspace.
*   **Jujutsu:** The modern version control system used in this workflow.
*   **Docker:** For containerizing applications. [Docker Desktop](https://www.docker.com/products/docker-desktop/) is a recommended starting point.

---

## Step 2: Install VS Code Extensions

These extensions provide the core functionality for our environment.

*   **ESLint** (`dbaeumer.vscode-eslint`): The standard for identifying and fixing problems in JavaScript and TypeScript code for the Node.js workspace.
*   **Prettier - Code formatter** (`esbenp.prettier-vscode`): An opinionated code formatter to ensure consistent style in the Node.js workspace.
*   **Error Lens** (`usernamehw.errorlens`): Makes errors and warnings more visible by printing them directly on the line.
*   **Deno** (`denoland.vscode-deno`): The official all-in-one extension for Deno. It handles formatting, linting, and TypeScript integration, replacing ESLint and Prettier in the Deno workspace.
*   **Docker** (`ms-azuretools.vscode-docker`): Simplifies the creation, management, and debugging of containerized applications.
*   **GitLens — Git supercharged** (`eamodio.gitlens`): Massively enhances VS Code's built-in Git capabilities. It provides powerful history navigation and insights that work perfectly with a `jj`-managed Git repository.
*   **Markdown Mermaid** (`bierner.markdown-mermaid`): Renders Mermaid diagrams and flowcharts in VS Code's Markdown preview.

---

## Step 3: Create the Workspaces

Create two `.code-workspace` files in your main source code directory. These files define the settings for each environment.

### A. Node.js Workspace

This is for all Node.js and general-purpose projects. It enables ESLint and Prettier and disables Deno's tools.

**File:** `nodejs-projects.code-workspace`
```json
{
	"folders": [
		// Add your Node.js project folders here
		// Example:
		// {
		// 	"path": "./path/to/your/node-project"
		// }
	],
	"settings": {
		"editor.defaultFormatter": "esbenp.prettier-vscode",
		"editor.formatOnSave": true,
		"eslint.enable": true,
		"deno.enable": false,
		"deno.lint": false
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

## Step 4: Version Control Workflow

This environment uses Jujutsu (`jj`) for a more powerful and intuitive version control experience, with Git acting as the backend for compatibility with GitHub.

For a detailed guide on the day-to-day workflow, see the Jujutsu Workflow Guide.md.