# Installation & Setup Guide

This guide provides instructions to install and set up **pdf-compiler** (`image_to_pdf`), either automatically via an **AI Agent** or **manually**.

---

## 🤖 Option 1: AI Agent Setup (Copy & Paste Prompt)

If you are using an AI coding assistant (such as **Claude Code**, **Cursor**, **Windsurf**, **Antigravity**, **GitHub Copilot**, or **Aider**), simply copy the prompt below and paste it into your AI assistant:

```markdown
You are setting up the `pdf-compiler` repository on my machine. Please perform the following steps autonomously:

1. **Environment Check**:
   - Verify Python version (requires Python >= 3.10).
   - Check if `uv` package manager is available (`uv --version`). If not installed, you can either install it via `curl -LsSf https://astral.sh/uv/install.sh | sh` (or `pip install uv`), or fall back to standard `venv` + `pip`.

2. **Dependency & Environment Setup**:
   - **If using `uv` (preferred)**:
     - Run `uv sync` to install all runtime and development dependencies.
     - Install the CLI command globally/editable so `image_to_pdf` is accessible from anywhere in the terminal:
       `uv tool install --editable . --force`
   - **If using standard `pip`**:
     - Create and activate a virtual environment (`python3 -m venv .venv && source .venv/bin/activate`).
     - Install package and dev dependencies: `pip install -e ".[dev]"`

3. **Verification**:
   - Run the test suite to ensure all unit tests pass:
     `uv run python -m pytest` (or `pytest`)
   - Verify the CLI binary is executable:
     `image_to_pdf --help`

4. **Completion Report**:
   - Report if tests passed.
   - Show the detected install path of `image_to_pdf`.
   - Provide a quick 1-line command example showing how I can run `image_to_pdf` on a sample folder or PDF name.
```

---

## 👤 Option 2: Human Manual Installation

### 1. Prerequisites
- **Python 3.10** or higher
- **Git**
- [`uv`](https://docs.astral.sh/uv/) (strongly recommended) or standard `pip`

---

### 2. Clone the Repository

```bash
git clone https://github.com/<your-username>/pdf-compiler.git
cd pdf-compiler
```

---

### 3. Setup Dependencies

#### Method A: Using `uv` (Fast & Recommended)

1. **Install project dependencies:**
   ```bash
   uv sync
   ```

2. **Install `image_to_pdf` as a global terminal command:**
   ```bash
   uv tool install --editable . --force
   ```
   *This makes `image_to_pdf` accessible from any terminal window and directory!*

---

#### Method B: Using Standard Python & `pip`

1. **Create and activate a virtual environment:**
   - **Linux / macOS:**
     ```bash
     python3 -m venv .venv
     source .venv/bin/activate
     ```
   - **Windows (Command Prompt / PowerShell):**
     ```cmd
     python -m venv .venv
     .venv\Scripts\activate
     ```

2. **Install package in editable mode with development tools:**
   ```bash
   pip install -e ".[dev]"
   ```

---

### 4. Verify Installation

1. **Run tests to ensure everything is working properly:**
   - With `uv`:
     ```bash
     uv run python -m pytest
     ```
   - With standard `venv`:
     ```bash
     pytest
     ```
   *(All 24 unit tests should pass).*

2. **Check the CLI command:**
   ```bash
   image_to_pdf --help
   ```

3. **Test compilation (Quick Smoke Test):**
   ```bash
   image_to_pdf "test_output.pdf"
   ```

---

## 🔧 Troubleshooting

### `image_to_pdf: command not found`
If you installed via `uv tool install` but the terminal cannot find `image_to_pdf`, ensure that your user binary path is added to your shell `$PATH`:
- **Linux/macOS**: Add `export PATH="$HOME/.local/bin:$PATH"` to `~/.bashrc` or `~/.zshrc`.
- Alternatively, you can always run it directly from the repo using:
  ```bash
  uv run image_to_pdf <PDF_NAME>
  # or
  python main.py <PDF_NAME>
  ```
