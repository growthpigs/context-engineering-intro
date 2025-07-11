# Claude-Specific Usage Notes

## 🚨 CRITICAL: Preventing Broken Links

**EVERY TIME you create or import files, you MUST:**

1. **Use lowercase-kebab-case for ALL files**: `user-profile.tsx` NOT `UserProfile.tsx`
2. **Include extensions in ALL imports**: `import { Button } from './button.tsx'` NOT `from './button'`
3. **Use path aliases for deep imports**: `import { useAuth } from '@/hooks/use-auth'` NOT `'../../../hooks/useAuth'`
4. **Verify imports before committing**: Check that every import path actually exists
5. **Update ALL imports when renaming**: Use Find & Replace across entire project

**Common Mistakes That Break Everything:**
- ❌ `import { Dashboard } from './Dashboard'` (missing .tsx, wrong case)
- ❌ `import { userService } from '../services/UserService'` (wrong case)
- ❌ Creating `UserProfile.tsx` then importing `'./user-profile'`
- ✅ `import { Dashboard } from './dashboard.tsx'` (correct!)

**Before EVERY commit, run this mental checklist:**
- Are all my files lowercase-kebab-case?
- Do all my imports have extensions?
- Did I update imports after moving/renaming?
- Do all import paths actually exist?

---

## Original Python Project Guidelines

### 🔄 Project Awareness & Context
- **Always read `PLANNING.md`** at the start of a new conversation to understand the project's architecture, goals, style, and constraints.
- **Check `TASK.md`** before starting a new task. If the task isn't listed, add it with a brief description and today's date.
- **Use consistent naming conventions, file structure, and architecture patterns** as described in `PLANNING.md`.
- **Use venv_linux** (the virtual environment) whenever executing Python commands, including for unit tests.

### 🧱 Code Structure & Modularity
- **Never create a file longer than 500 lines of code.** If a file approaches this limit, refactor by splitting it into modules or helper files.
- **Organize code into clearly separated modules**, grouped by feature or responsibility.
  For agents this looks like:
    - `agent.py` - Main agent definition and execution logic 
    - `tools.py` - Tool functions used by the agent 
    - `prompts.py` - System prompts
- **Use clear, consistent imports** (prefer relative imports within packages).
- **Use clear, consistent imports** (prefer relative imports within packages).
- **Use python_dotenv and load_env()** for environment variables.

### 🧪 Testing & Reliability
- **Always create Pytest unit tests for new features** (functions, classes, routes, etc).
- **After updating any logic**, check whether existing unit tests need to be updated. If so, do it.
- **Tests should live in a `/tests` folder** mirroring the main app structure.
  - Include at least:
    - 1 test for expected use
    - 1 edge case
    - 1 failure case

### ✅ Task Completion
- **Mark completed tasks in `TASK.md`** immediately after finishing them.
- Add new sub-tasks or TODOs discovered during development to `TASK.md` under a "Discovered During Work" section.

### 📎 Style & Conventions
- **Use Python** as the primary language.
- **Follow PEP8**, use type hints, and format with `black`.
- **Use `pydantic` for data validation**.
- Use `FastAPI` for APIs and `SQLAlchemy` or `SQLModel` for ORM if applicable.
- Write **docstrings for every function** using the Google style:
  ```python
  def example():
      """
      Brief summary.

      Args:
          param1 (type): Description.

      Returns:
          type: Description.
      """
  ```

### 📚 Documentation & Explainability
- **Update `README.md`** when new features are added, dependencies change, or setup steps are modified.
- **Comment non-obvious code** and ensure everything is understandable to a mid-level developer.
- When writing complex logic, **add an inline `# Reason:` comment** explaining the why, not just the what.

### 🧠 AI Behavior Rules
- **Never assume missing context. Ask questions if uncertain.**
- **Never hallucinate libraries or functions** – only use known, verified Python packages.
- **Always confirm file paths and module names** exist before referencing them in code or tests.
- **Never delete or overwrite existing code** unless explicitly instructed to or if part of a task from `TASK.md`.