# Repository Guidelines

Use this guide to keep WordPress contributions predictable, reviewable, and ready for handoff between agents. Keep updates aligned with the current spec and document any intentional deviations.

## Project Structure & Module Organization
- Store WordPress code inside `wp-content/` using the standard layout: themes in `wp-content/themes/<theme-slug>/`, plugins in `wp-content/plugins/<plugin-slug>/`, and mu-plugins under `wp-content/mu-plugins/`.
- Group shared PHP services and stateful logic inside `includes/` or `src/`, and mirror that structure under `tests/` for PHPUnit coverage.
- Place front-end assets in `assets/` with separate `js/`, `css/`, and `images/` folders. Generate build artifacts into `build/` (do not commit vendor bundles unless the deployment workflow requires them).
- Centralize operational docs and agent briefs in `docs/` (e.g., `docs/implementation-agent.md`). Update them whenever behavior or assumptions change so other roles stay in sync.

## Build, Test, and Development Commands
- Install PHP dependencies with Composer and JavaScript tooling with npm:
```bash
composer install
npm install
```
- Run automated checks before review:
```bash
npm run build       # Compile assets or block editor code
npm run lint        # Aggregate JS/Sass lint commands
composer run lint   # PHPCS using WordPress Coding Standards
composer run test   # PHPUnit suite (set WP tests DB credentials first)
```
- When the feature alters runtime behavior, execute the relevant WP-CLI task (`wp cron event run`, `wp option get`) to confirm state changes instead of relying on assumptions.

## Coding Style & Naming Conventions
- Follow WordPress Coding Standards (WPCS) with four-space indentation for PHP, snake_case function names, and PascalCase for classes. Type-hint where supported and add docblocks that clarify filters, actions, and expected data shapes.
- Keep React or ES modules aligned with the Gutenberg package structure; default to camelCase for variables and PascalCase for components. Reuse shared helpers and avoid bypassing established state stores.
- SCSS should use BEM naming, and group design tokens in `_settings.scss`. Record any global stylesheet changes in the PR description.

## Testing Guidelines
- Maintain PHPUnit coverage for business logic, naming tests after the scenario (`test_handles_empty_cart_gracefully`). Mock WordPress APIs only when integration is impractical; otherwise exercise code through WP test factories.
- Capture manual validation steps—such as visiting the modified admin screen or running a background cron—in `docs/validation.md` so other agents can replay them.
- Prefer background jobs or scheduled WP-Cron events for recurring fixes rather than runtime hooks, and document their trigger cadence.

## Commit & Pull Request Guidelines
- Use Conventional Commits with imperative subjects (`feat: add cart recovery cron`). Reference tickets or specs in the body and note any assumptions flagged during implementation.
- PRs must include scope, screenshots or screencasts for UI changes, affected hooks or options, and the commands executed (`npm run build`, `composer run test`, manual cron runs). Link related agent briefs or follow-up issues.
- Before requesting review, confirm documentation updates, list outstanding risks, and surface next steps for the Debugging or QA agents when relevant.
