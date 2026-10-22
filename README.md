# CodeScan — AI Code Reviewer & Bug Detection System
## Setup (Windows PowerShell)
    python -m venv venv; .\venv\Scripts\Activate.ps1
    pip install -r requirements.txt; copy .env.example .env; python app.py
## Linux/macOS
    python3 -m venv venv && source venv/bin/activate
    pip install -r requirements.txt && cp .env.example .env && python app.py
Open http://127.0.0.1:5000. Set a real SECRET_KEY in `.env`.
## AI (optional)
Put `AI_API_KEY`, `AI_MODEL`, `AI_API_URL` in `.env` (server-side only). The endpoint must accept OpenAI-style chat-completions requests. Without them, static analysis still runs.
## Tests
    pytest -q
## Production
Use `FLASK_ENV=production`, a strong SECRET_KEY, PostgreSQL `DATABASE_URL` (+ psycopg2), HTTPS, `gunicorn "app:create_app()"`, and a shared rate-limit store (Redis) instead of the in-memory one.

## Suggested Code
After each review, CodeScan builds a corrected version of your code (automatic fixes, upgraded by the AI when configured). The AI version is only shown if static analysis rates it at least as good. A diff and before/after score are included; nothing overwrites your editor unless you click "Load into editor".
## Output
"Run original / Run suggested" executes Python, JavaScript, Bash, Ruby and PHP (if installed) and shows stdout/stderr; HTML shows a sandboxed preview. This is NOT a hardened sandbox, so it is off unless FLASK_ENV=development or ENABLE_CODE_EXECUTION=true. Never enable it on a public server.

## Analysis engines and accuracy
Syntax errors come from real parsers (Python `ast`, tree-sitter for JS/TS/Java/C/C++/C#/Go/PHP/Ruby/Rust/Bash/Kotlin/Swift, plus built-in CSS and HTML parsers), so they are essentially exact. Security/bug findings are rule-based and carry a Confirmed/Potential/Suggestion label; expect roughly 80-90% useful results, not perfection. SQL has no parser (rules only). If a tree-sitter package is missing, that language falls back to pattern rules; the review status line shows which engine was used. `tests/corpus.py` holds the clean/buggy samples that guard accuracy: add a failing sample there whenever you find a miss.

## Accounts, reports and settings
- **Login / register** with username or email and a password.
- **Reports** download as a PDF with scores, severity chart, findings, source listing and suggested code. **Import file** reads a source file in the browser (max 200 KB).
- **Profile** shows your account data and stats; **Settings** has profile, password, full history and account deletion.
