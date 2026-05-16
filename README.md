# Zero Hour

## Educational Purpose

This project was created primarily for **educational and learning purposes**.  
While it is well-structured and could technically be used in production, it is **not intended for commercialization**.  
The main goal is to explore and demonstrate best practices, patterns, and technologies in software development.

## Description

**Zero Hour** is a lightweight, zero-dependency countdown web application built with vanilla TypeScript and TailwindCSS. It displays a real-time countdown timer that ticks every second toward a target date and time, making it ideal for embedding in product launch pages, giveaway campaigns, game release announcements, or any scenario where building anticipation matters.

The UI centers on a styled card that shows the target event's full date alongside four live countdown units — Days, Hours, Minutes, and Seconds — each updating automatically as time passes. When the countdown reaches zero, the timer stops and the card transitions to an expiration message, signaling that the offer or event window has closed.

Under the hood, the app uses a generic reactive `Store<T>` class for state management, a component model based on plain TypeScript classes that return DOM elements, and a `setInterval`-driven render loop. The architecture is intentionally minimal: no frameworks, no runtime dependencies, just typed TypeScript composed into a single-page application bundled by Vite. The project also includes a full test suite with Jest, Testing Library, and 70%+ coverage enforced on every build.

## Technologies used

1. Typescript
2. TailwindCSS
3. HTML5
4. CSS3
5. Vite

## Libraries used

The project relies exclusively on development tooling — there are no runtime dependencies shipped to the browser.

#### Dependencies

```
No production dependencies - Pure Vanilla TypeScript
```

#### devDependencies

```
"@eslint/js": "^9.39.2"
"@testing-library/dom": "^10.4.0"
"@testing-library/jest-dom": "^6.6.3"
"@testing-library/user-event": "^14.5.2"
"@types/jest": "^30.0.0"
"@types/node": "^22.0.0"
"autoprefixer": "^10.4.16"
"eslint": "^9.39.2"
"eslint-config-prettier": "^10.1.8"
"eslint-plugin-prettier": "^5.5.5"
"globals": "^17.3.0"
"husky": "^9.1.7"
"jest": "^30.3.0"
"jest-environment-jsdom": "^30.3.0"
"lint-staged": "^16.2.7"
"postcss": "^8.4.33"
"prettier": "^3.8.1"
"tailwindcss": "^3.4.1"
"ts-jest": "^29.4.6"
"typescript": "^5.2.2"
"typescript-eslint": "^8.54.0"
"vite": "^7.1.5"
```

## Getting Started

Follow these steps to run the project locally:

1. Clone the repository
2. Navigate to the project folder
3. Use Node.js 22 (a `.nvmrc` is provided — run `nvm use` if you have nvm)
4. Execute: `npm install`
5. Execute: `npm run dev`

The application will open automatically at `http://localhost:3000`

## Testing

Once the project is installed, you can run the full test suite to verify everything works as expected:

1. Navigate to the project folder
2. Execute: `npm test`

For coverage report:

```bash
npm run test:coverage
```

## Continuous Integration

The repository ships with a **GitHub Actions** pipeline defined in [`.github/workflows/ci.yml`](.github/workflows/ci.yml). It runs automatically on every `push` and `pull_request` targeting the `main` branch.

### Pipeline overview

```
                      ┌─── PR or push to main ───┐
                      ▼                          ▼
┌──────────────────────┐  ┌──────────────────┐  ┌──────────────────┐
│    lint-and-audit    │─▶│     testing      │─▶│      build       │
│ eslint · type-check  │  │   jest (jsdom)   │  │   vite build     │
└──────────────────────┘  └──────────────────┘  └──────────────────┘
```

### Validation jobs (run on every PR and push)

1. **`lint-and-audit`** — installs dependencies with `npm ci`, runs `npm run lint` (ESLint) and `npm run type-check` (TypeScript `--noEmit`).
2. **`testing`** — runs `npm run test`, executing the full Jest suite under `jest-environment-jsdom` with the 70% coverage threshold enforced by `jest.config.js`.
3. **`build`** — runs `npm run build`, which performs `tsc -b` followed by `vite build` and emits the production bundle to `dist/`. Acts as a smoke test that the project compiles end-to-end.

Every job runs on `ubuntu-latest`, checks out the repo with `actions/checkout@v4.2.2`, sets up Node.js via `actions/setup-node@v4` using the version pinned in [`.nvmrc`](.nvmrc), and caches the npm registry to keep installs fast.

### Job dependencies

Jobs run **sequentially** via the `needs:` keyword: `testing` waits for `lint-and-audit` to succeed, and `build` waits for `testing`. A failure in any earlier stage short-circuits the pipeline and prevents later jobs from running.

### Skipping CI

To push a change to `main` without triggering the workflow (e.g. a typo fix in an unrelated doc), append GitHub's standard `[skip ci]` marker to the commit message:

```bash
git commit -m "docs: fix typo in README [skip ci]"
```

### Where the build outputs live

| Output                         | Location                     |
| ------------------------------ | ---------------------------- |
| Lint, type-check and test logs | **Actions** tab on GitHub    |
| Production bundle (`dist/`)    | Ephemeral, inside the runner |

> **Note:** the pipeline does not currently publish artifacts or releases — `npm run build` runs purely as a verification step.

### Running the same checks locally

```bash
# lint-and-audit
npm run lint
npm run type-check

# testing
npm run test

# build
npm run build
```

## Security Audit

Beyond functional tests, you should also check the dependency tree for known vulnerabilities.

### npm audit

Check for vulnerabilities in dependencies:

```bash
npm audit
```

## Known Issues

None at the moment.

## Portfolio Link

[`https://www.diegolibonati.com.ar/#/project/zero-hour`](https://www.diegolibonati.com.ar/#/project/zero-hour)
