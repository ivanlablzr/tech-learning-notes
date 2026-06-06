---
type: note
tags: [programming, languages, web, data]
---


The languages you'll actually write, plus the two biggest applied tracks built on them — **web development** and **data analytics**.

## 1. Core languages

| Language | Typical use | Notes |
|---|---|---|
| **Python** | Scripting, data/ML, automation, backends | Readable; the data & security workhorse |
| **JavaScript / TypeScript** | The browser (only native web language) + Node backends | TS adds compile-time type checking |
| **HTML / CSS** | Web structure & style (markup, not programming) | Reference: MDN |
| **SQL** | Querying relational data | Essential even for "Python" analysts |
| **Bash / PowerShell** | OS automation, glue | See [[Linux]] / [[Windows]] |
| **Go / Rust / Java** | Fast services, systems, enterprise | Go = simple concurrency; Rust = memory-safe perf |

**Python in one screen:** types (int/float/str/bool) and structures (list, set, tuple, **dict** = key→value); control flow (`for`/`while`/`if-elif-else`/`match`); functions (default/keyword/`*args`/`**kwargs`, scope, lambdas, higher-order); operators (arithmetic, comparison, logical, bitwise, membership `in`, identity `is`); **mutable vs immutable**; file I/O; then OOP (classes/objects), decorators, generators, exception handling, virtual environments. Good habits: meaningful names, no hard-coded values, comprehensions, error handling, `venv` per project.

> The exhaustive HTML element/attribute lists are reference material — use **MDN** rather than memorizing. The structural essentials are in §2.

## 2. Web development

Every page rests on three browser-native pillars: **HTML** (structure/semantics), **CSS** (style — Flexbox for 1-D, Grid for 2-D, media queries for responsive), **JavaScript** (behavior — DOM, events, `fetch`/async-await, ES6+).

**Frontend frameworks:** **React** (component library, hooks, huge ecosystem — the default), Vue (gentler), Svelte (compiles away its runtime), Angular (full enterprise framework). **Meta-frameworks** add SSR/routing: **Next.js** (React), Astro (content/islands), Nuxt (Vue), SvelteKit. **Rendering strategies:** SSR (per-request HTML, good SEO), SSG (pre-built, fastest), ISR (background regen), CSR (classic SPA), islands (minimal JS). Tooling: **Vite** (dev server), Tailwind (utility CSS), npm/pnpm.

**Backend** receives requests → authenticates → runs business logic → reads/writes a database → responds.

| Language | Frameworks | Strength |
|---|---|---|
| Node.js | Express, Fastify, Hono | Same language as frontend |
| Python | FastAPI, Django, Flask | Data/ML-adjacent; FastAPI is fast + typed |
| Go / Rust | Gin / Axum | Performance, concurrency |

**APIs:** **REST** (resources over HTTP+JSON — the default), **GraphQL** (client picks fields, avoids over/under-fetching), **tRPC** (end-to-end type-safe TS). **Databases:** PostgreSQL (default), SQLite (embedded), MongoDB (document), Redis (in-memory cache/sessions) — accessed via ORMs (Prisma, SQLAlchemy). Deep dive: [[08 Databases]].

**Auth & sessions:** prefer **HttpOnly, SameSite cookies** over JWTs in localStorage (XSS risk); **OAuth 2.0** for "login with…" (Authorization Code / PKCE); hash passwords with **bcrypt/Argon2id**.

**HTTP essentials:** methods (GET idempotent/cacheable, POST creates, PUT/PATCH/DELETE), status codes (2xx ok, 3xx redirect, 4xx client, 5xx server), **HTTP/2** (multiplexing) vs **HTTP/3** (QUIC over UDP, 0-RTT), and **CORS** (the controlled relaxation of the same-origin policy).

**Web security (OWASP-aligned):** parameterize queries (SQLi), escape output + CSP (XSS), SameSite/CSRF tokens (CSRF), authorize every request (IDOR), HTTPS everywhere. Cross-reference [[Internet & Application Security]].

**Deploy:** static (Vercel/Netlify/Cloudflare Pages), PaaS (Railway/Render/Fly), VPS (DigitalOcean/Hetzner), or containers ([[10 DevOps|Docker/K8s]]). Workflow: Git + GitHub Actions CI/CD, ESLint/Prettier, Jest/Playwright tests, secrets in env vars (never commit `.env`).

## 3. Data analytics

Turning raw data into decisions. The maturity ladder: **descriptive** (what happened) → **diagnostic** (why) → **predictive** (what will) → **prescriptive** (what to do). Pipeline: **collect → store → process (ETL/ELT) → analyze → visualize → decide**.

**Python stack:** **pandas** (DataFrames — `groupby`, `merge`, `pivot_table`, boolean indexing), **NumPy** (fast arrays, vectorized ops), **Matplotlib/Seaborn** (plots), **Plotly** (interactive), **Jupyter** (EDA). **SQL** is still essential — joins (INNER/LEFT/FULL), **window functions** (`ROW_NUMBER/RANK/LAG OVER (PARTITION BY…)`), and **CTEs** for readable complex queries.

**Statistics:** central tendency (mean sensitive to outliers, median robust), spread (variance, σ, IQR), distributions (normal, Poisson, binomial), **correlation ≠ causation**, hypothesis testing (H₀/H₁, p<0.05, t-test/chi-squared/ANOVA), regression (linear; logistic = classification).

**Data cleaning** eats 60–80% of the work: handle missing values (drop/impute/forward-fill), detect outliers (z-score ±3σ, IQR), normalize types/strings, deduplicate. **Visualization:** match chart to question (bar=compare, line=trend, scatter=correlation, histogram=distribution, heatmap=matrix); maximize data-ink, don't truncate axes.

**ML for prediction (scikit-learn):** classification/regression/clustering/dimensionality-reduction; **feature engineering** (encoding, scaling); evaluate with **precision/recall/F1/AUC-ROC** (classification — accuracy lies on imbalanced data) and MAE/RMSE/R² (regression); use **k-fold cross-validation**. Deeper ML in [[Machine Learning & Deep Learning]].

**Scale & tooling:** **Spark** (distributed in-memory), warehouses (Snowflake/BigQuery/Redshift — columnar OLAP), lakes (S3 + Parquet, Delta/Iceberg), **dbt**/Airflow for ELT. BI dashboards: Tableau, Power BI, Metabase, Grafana. **Security use cases:** log analysis, anomaly/threat detection, SIEM dashboards, UEBA — analytics is directly applicable to a SOC.

Related: [[Programming Foundations]] · [[07 Programming|domain overview]] · [[08 Databases]] · [[14 AI]] · [[Internet & Application Security]]
