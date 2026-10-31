# Guide

---

## Content Editor's Guide

This guide is for updating the educational content of the application — no coding experience required. There are two files you can safely edit. Everything else should be left alone (Unless it's contributed by someone who understands the codebase. See other simple guide for additional information).

---

### The Two Files You Can Edit

#### 1. `src/data/shardContent.tsx` — The Lesson Text

This file contains all the main narrative text that learners read in each lesson (called a "shard"). There are 9 shards, numbered 1 through 9.

**What you can change:** The words inside `<p>` tags, and the words inside `<strong>` tags. These are the paragraphs and bold phrases learners see on screen.

**Example — changing a paragraph:**

```
Before:
<p>
    In the 17th Century, Dutch settlers arrived in South Africa,
    followed by the British in the 18th Century.
</p>

After:
<p>
    In the 17th Century, Dutch colonisers arrived in South Africa,
    followed by the British in the 18th Century.
</p>
```

Just change the words between the `<p>` and `</p>`. Do not delete the tags themselves.

**What you must NOT touch:**
- Lines that start with `import` (at the very top of the file)
- Anything that looks like `<GlossaryTerm word="..." />` — these are links to glossary definitions and must stay exactly as they are
- The opening `<div>` and closing `</div>` tags that wrap each shard
- The numbers `1:`, `2:`, `3:` etc. that label each shard
- The last line: `export default shardContent;`
- Any curly braces `{ }` or parentheses `( )`

---

#### 2. `src/data/glossary.ts` — The Glossary Definitions

This file contains definitions for every term that appears as a clickable glossary link in the lessons. Each line follows this format:

```
"Term": "Definition text here.",
```

**Example — updating a definition:**

```
Before:
"Apartheid": "A system of institutionalised racial segregation enforced in South Africa from 1948 to 1994.",

After:
"Apartheid": "A system of institutionalised racial segregation enforced in South Africa from 1948 to 1994, separating people by race in nearly every aspect of daily life.",
```

Only change the text between the second pair of quote marks (the definition). Do not change the term name on the left — it must stay exactly the same, as it is referenced in the lesson text.

**What you must NOT touch:**
- The term names on the left side (e.g. `"Apartheid"`, `"Robben Island"`)
- The colon `:` between the term and definition
- The comma `,` at the end of each line
- The first line: `const glossary: Record<string, string> = {`
- The last line: `export default glossary;`
- Comment lines beginning with `//` (e.g. `// Shard 1`) — these are just labels for your reference

---

### A Safe Way to Edit (BACKUP)

Don't be frightened with changing things around in Content Editor's Guidelines.
You also don't need any sort of backup before changing because the codebase is safely
stored in the creators GitHub repository: [AALC-Interactive Puzzle Timeline Repo](https://github.com/SujitBhatta21/AALC-IndividualProject)

---

### Files to Never Edit

Everything else in the project — especially anything inside '`server/`', '`src/components/`, `src/pages/`, or `src/styles/` — contains application code. Editing those files can break the application in ways that are difficult to undo.

---

---

## Codebase Editor's Guide

This guide is for a developer who is familiar with React and TypeScript but is new to this codebase. It maps every visible page and UI area to the files responsible for it.

---

### Pages

Each page lives in `src/pages/` and has a matching stylesheet in `src/styles/`.

| Page / Screen | File | Route | Stylesheet |
|---|---|---|---|
| Home (user view) | `src/pages/HomePage.tsx` | `/` | `src/styles/HomePage.css` |
| Admin dashboard | `src/pages/AdminPage.tsx` | `/` (rendered inside HomePage when role is ADMIN) | `src/styles/AdminPage.css` |
| Timeline / Shard map | `src/pages/TimelinePage.tsx` | `/storyline` | `src/styles/TimelinePage.css` |
| Individual shard experience | `src/pages/ShardPage.tsx` | `/storyline/shard/:id` | `src/styles/Puzzle.css` |
| Login & registration | `src/pages/LoginPage.tsx` | `/accounts/login` | `src/styles/LoginPage.css` |
| Who We Are (about AALC) | `src/pages/WhoWeAre.tsx` | `/who-we-are` | `src/styles/WhoWeAre.css` |
| Settings | `src/pages/Settings.tsx` | `/settings` | `src/styles/Settings.css` |

All routes that require a login redirect to `/accounts/login` if no token is found in localStorage.

---

### Shared Components

These components appear across multiple pages and live in `src/components/`.

| Component | File | What it does |
|---|---|---|
| Navigation bar | `src/components/Header.tsx` | Top nav with logo, user avatar, logout, and page links. Stylesheet: `src/styles/Header.css` |
| Footer | `src/components/Footer.tsx` | Site footer with address, email, and social links. Stylesheet: `src/styles/Footer.css` |
| Glossary tooltip | `src/components/GlossaryTerm.tsx` | Inline term that shows a definition on click. Styled by `src/styles/GlossaryTerm.css` |
| Reward popup | `src/components/RewardPopup.tsx` | Celebration screen shown after completing a puzzle |
| Final message | `src/components/FinalMessage.tsx` | Completion screen shown after Shard 9 |
| Bug report form | `src/components/ReportForm.tsx` | Modal form in Settings > About for submitting issues |

---

### Puzzle Components

Each puzzle type is its own component in `src/components/puzzles/`. `ShardPage.tsx` selects which one to render based on the `puzzleType` value stored for each shard.

---

### Data & Types

| File | What it contains |
|---|---|
| `src/data/shardContent.tsx` | JSX content (narrative text) for all 9 shards |
| `src/data/glossary.ts` | Glossary term definitions referenced by GlossaryTerm |
| `src/types.ts` | TypeScript interfaces for Shard, User, Report, and the SIZE_MAP constant |
| `src/utils.ts` | Shared utility functions used across the app |
| `src/App.tsx` | Root component — defines all routes and wraps the app |
| `src/App.css` | Global base styles |

---

### Where to Look for What

- **A page looks wrong?** Start with the page file in `src/pages/` and its matching CSS in `src/styles/`.
- **The nav or footer has an issue?** Go to `src/components/Header.tsx` or `src/components/Footer.tsx`.
- **A puzzle is broken?** Go to the relevant file in `src/components/puzzles/`.
- **A glossary term shows the wrong definition?** Edit `src/data/glossary.ts`.
- **The lesson text is wrong?** Edit `src/data/shardContent.tsx`.
- **A TypeScript type error?** Check `src/types.ts` first.
- **Routing is broken?** Check `src/App.tsx`.

---

---

## Server Codebase Guide

The server is a Java Spring Boot app. All source files live under `server/src/main/java/com/example/server/`. It follows standard Spring MVC layering: every feature touches four packages in order — controller → service → repository → model.

| Package | What lives here |
|---|---|
| `controller/` | REST endpoints. Each file maps to a URL prefix: `UserController` → `/api/accounts/*`, `ShardController` → `/api/shards/*`, `ReportController` → `/api/reports/*`, `AdminController` → admin-only stats and user management |
| `service/` | Business logic. Controllers call services; services never talk to each other or to controllers directly |
| `repository/` | Database queries via Spring Data JPA. Add custom queries here as interface methods |
| `model/` | JPA entities (database tables): `User`, `Shard`, `UserShardProgress`, `Report`. Also holds enums (`Role`, `ReportStatus`) and the `LoginResponse` DTO |
| `config/` | `SecurityConfig.java` — JWT rules and route permissions. `CorsConfig.java` — allowed frontend origins. `DataSeeder.java` — seeds default data on startup |
| `converter/` | `FitbAnswerConverter.java` — serialises the fill-in-the-blank answer format to/from the database |

**Key config file:** `server/src/main/resources/application.properties` — database URL, credentials, JWT secret, and port are all set here (via environment variables).

### Where to Look for What

- **An API endpoint is wrong or missing?** Start in `controller/`, then follow the call into `service/`.
- **Wrong data coming back from the database?** Check the `repository/` query, then the `model/` entity mapping.
- **Authentication or CORS issues?** Go to `config/SecurityConfig.java` or `config/CorsConfig.java`.
- **Seed data is incorrect?** Edit `config/DataSeeder.java`.
- **Database column or relationship change?** Update the entity in `model/` and the matching repository.