# Repository Guidelines

## 1. Project Structure & Module Organization
This Divi child theme runs from `wp-content/themes/Divi-Child-Theme`. `functions.php` bootstraps feature modules and registers cron jobs. Domain logic is grouped under `includes/`, covering booking flows, partner tooling, and the Eversports synchronisation helpers. Front-end assets live in `assets/css`, `assets/js`, and `assets/img`; add global behaviour in `script.js` and keep feature-specific assets beside their PHP module. Custom templates belong in `template/`, while WooCommerce overrides sit in `woocommerce/`. Capture larger specs or UX notes alongside `spec.md` for future contributors.

## 2. Build, Test, and Development Commands
There is no build pipeline yet; the theme loads directly via WordPress. Useful WP-CLI commands in a Local or staging environment:
- `wp cache flush` — clear WordPress caches after editing templates or queries.
- `wp transient delete --all` — reset cached Eversports payloads before re-testing calendars.
- `wp action-scheduler run --hook=execute_sync_update_studios` — trigger the studio sync job on demand.

## 3. Coding Style & Naming Conventions
- Follow PHP coding standards detailed at https://github.com/piotrplenik/clean-code-php
- Use PHPCS and PHPCBF to find and fix syntax errors
- Prefix custom hooks and helpers with `zs_` for clarity
- Use ES6 for JavaScript and use jQuery whenever possible
- Prefix CSS names with `zs-` and use the BEM naming convention.
- Equal signs do not need to line up vertically

## 4. Testing Guidelines
- Automated tests are not present yet
- Write tests for tricky code after implementation to check your work.

## 5. Commit & Pull Request Guidelines
- **Very important:** multiple developers are working at the same time in this repository

### 5.1. Core Principle: Atomic Commits
Every commit must represent **one logical, self-contained change**.
This means:
- Each commit solves exactly one problem or implements one clear sub-feature
- Commits should be **independent**, **reversible**, and **reviewable** in isolation
- Avoid bundling unrelated edits (e.g., formatting, refactoring, and feature code) into the same commit

### 5.2. Work incrementally
- ALWAYS keep commits atomic and focused
- Break each feature or task into a sequence of **clear, logical steps**
- Treat each step as a separate "atomic work unit" to be committed independently

### 5.3. During editing
- Only modify the files strictly necessary for the current sub-task.
- Do not pre-optimize or refactor unrelated parts of the codebase.

### 5.4. Testing before committing
- After writing code, run the local tests or verification commands (e.g. `npm test`, `pytest`, or `go test ./...`).
- Proceed to commit **only if tests pass**.
- If tests fail, fix them within the same atomic commit scope.

### 5.5. Committing
- ALWAYS check `git status` before staging anything
- ALWAYS use specific file paths with `git add`: `git add /path/to/file`
- Stage only relevant files for the current change (`git add -p` preferred).
- Split unrelated changes into separate commits
- NEVER stage all files at once
- ONLY stage files you explicitly worked on
- NEVER touch the uncommitted files of other developers
- Begin subjects with the Jira ticket (i.e. `ZEN-1234`) which you'll usually find in the branch name
- Use the conventional commit framework for the commit message
- Write in imperative mood ("Add feature" not "Added feature")
- Explain why, not just what

## 6. Security & Configuration
External services rely on constants such as `EVERSPORTS_API_URL`, `EVERSPORTS_API_TOKEN`, and `STUDIO_SYNC_*`. Never commit secrets; store them in `wp-config.php`. Sanitize and escape user input where it makes sense.

## 7. Additional Notes
- When modifying `style.css`, bump the `Version` header so cached assets refresh. In development, add a fourth number (i.e. 1.3.26.1) and bump that. When a style change is approved, bump the third number (i.e. 1.3.27)
