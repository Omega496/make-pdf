# PDF Compiler

A fast, smart, and lightweight Python CLI tool and library for compiling images (`JPG`, `PNG`, `WEBP`, `BMP`, `TIFF`) into clean, beautifully formatted multi-page PDF documents.

---

## ✨ Features

- 📑 **Natural Numerical Sorting**: Sorts files naturally (e.g., `page1`, `page2`, `...`, `page10` instead of `page1`, `page10`, `page2`), preserving correct page order for grade cards, scans, and notes.
- 📱 **Auto EXIF Rotation**: Automatically rotates phone photos and camera scans using EXIF orientation metadata so pages are never sideways or upside-down.
- 🎨 **Smart Transparency Handling**: Transparent PNG/WEBP images are composited over a clean white background rather than rendering as black blocks.
- 🗂️ **Wide Image Format Support**: Works with `.jpg`, `.jpeg`, `.png`, `.webp`, `.bmp`, `.tiff`, and `.tif` files (case-insensitive, including `.JPG` and `.PNG`).
- 🗜️ **Compression & Optimization**: Control output file size using `--quality` (1–100) and automatic PDF stream optimization.
- 🛡️ **Robust Resource Management**: Clean file handle handling preventing file descriptor leaks, with options to skip corrupted files (`--skip-corrupted`).
- 💻 **Multiple Interfaces**: Use directly from the command line (`image_to_pdf` or `python main.py`) or import as a Python library into your own scripts.

---

## 🚀 Installation & Setup

### 🤖 AI Agent Quickstart (Copy & Paste Prompt)

If you are using an AI coding assistant (**Cursor**, **Claude Code**, **Windsurf**, **Antigravity**, **GitHub Copilot**, etc.), copy and paste this prompt into your AI agent:

````markdown
Set up the `pdf-compiler` repository in my current environment:

1. **Verify Environment**: Ensure Python >= 3.10 is installed. Check if `uv` is installed (`uv --version`).
2. **Install & Build**:
   - If `uv` is available (recommended):
     - Run `uv sync` to install dependencies.
     - Run `uv tool install --editable . --force` so the `image_to_pdf` CLI is available globally from any directory.
   - If using standard `pip`:
     - Create and activate a venv (`python3 -m venv .venv && source .venv/bin/activate`).
     - Run `pip install -e ".[dev]"`.
3. **Run Verification**:
   - Run the test suite: `uv run python -m pytest` (or `pytest`).
   - Verify CLI help: `image_to_pdf --help`.
4. **Confirm**: Report test results and show a quick 1-line example to run `image_to_pdf`.
````

---

### 👤 Manual Setup for Humans

#### Prerequisites
- Python **3.10** or higher
- [`uv`](https://docs.astral.sh/uv/) (strongly recommended) or standard `pip`

#### 1. Clone the repository
```bash
git clone https://github.com/<your-username>/pdf-compiler.git
cd pdf-compiler
```

#### 2. Install Dependencies

##### Option A: Using `uv` (Recommended)
```bash
# Sync dependencies
uv sync

# Install image_to_pdf as a global terminal command (run from ANY directory)
uv tool install --editable . --force
```

##### Option B: Using standard Python / `pip`
```bash
python3 -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
pip install -e ".[dev]"
```

#### 3. Verify Installation
```bash
image_to_pdf --help
uv run python -m pytest
```

> 📖 For detailed troubleshooting and environment tips, see [INSTALL.md](INSTALL.md).

---

## 📖 How to Use

### 1. Command Line Interface (CLI)

You can run `image_to_pdf` directly (or via `uv run image_to_pdf` or `python main.py`).

#### Basic Examples

- **Compile images in the current directory into a named PDF:**
  ```bash
  image_to_pdf "Grade_Cards.pdf"
  ```
  *(If no input folder is specified, `image_to_pdf` automatically searches your current working directory).*

- **Compile images from a specific directory:**
  ```bash
  image_to_pdf ./my_images -o "document.pdf"
  ```
  or simply:
  ```bash
  image_to_pdf ./my_images "document.pdf"
  ```

- **Compile with custom input and output flags:**
  ```bash
  image_to_pdf -i /home/user/scans -o /home/user/scans/compiled.pdf
  ```

- **Default run (compiles current directory to `output.pdf`):**
  ```bash
  image_to_pdf
  ```

---

#### Advanced Options & Flags

| Flag | Description | Default |
|------|-------------|---------|
| `-i`, `--input PATH` | Path to the directory containing images | Current directory |
| `-o`, `--output PATH` | Output PDF filename or path | `output.pdf` |
| `--sort {natural,alpha,none}` | Sorting method: `natural` (`page2` before `page10`), `alpha` (lexicographical), or `none` | `natural` |
| `--reverse` | Reverse the image ordering | `False` |
| `--quality N` | JPEG compression quality (1–100) to reduce PDF size | `None` (lossless/original) |
| `--no-optimize` | Disable Pillow PDF stream size optimization | Enabled |
| `--no-exif` | Disable automatic rotation based on EXIF camera metadata | Enabled |
| `--skip-corrupted` | Skip unreadable/corrupted files instead of stopping | `False` |
| `-v`, `--verbose` | Print detailed progress and list each image processed | `False` |
| `-q`, `--quiet` | Suppress all output except errors | `False` |
| `--help` | Show command line help and exit | — |
| `--version` | Display version number | — |

#### Practical Examples

- **Compress output PDF for email / uploads:**
  ```bash
  image_to_pdf ./grade_cards -o "Grades_Compressed.pdf" --quality 75
  ```

- **Compile in alphabetical order with verbose log:**
  ```bash
  image_to_pdf ./receipts -o "Receipts_2026.pdf" --sort alpha -v
  ```

- **Compile while skipping any broken image files:**
  ```bash
  image_to_pdf ./scans -o "Scans.pdf" --skip-corrupted
  ```

---

### 2. Using `main.py`

You can also run the script directly with Python:

```bash
python main.py "Grade_Cards.pdf"
python main.py ./scans -o "Scans.pdf" --quality 80
```

Or with `uv`:
```bash
uv run python main.py "Grade_Cards.pdf"
```

---

### 3. Using as a Python Library

You can import `pdf_compiler` directly into your Python scripts:

```python
from pdf_compiler import compile_folder, compile_images

# Example 1: Compile all images in a directory
pdf_path = compile_folder(
    folder_path="./documents",
    output_pdf="Compiled_Documents.pdf",
    sort_mode="natural",   # 'natural', 'alpha', or 'none'
    quality=85,            # Optional compression
    auto_rotate=True       # Auto-rotate phone camera photos
)
print(f"Generated PDF at: {pdf_path}")

# Example 2: Compile a custom list of image files
custom_images = [
    "page_intro.png",
    "page_content.jpg",
    "page_summary.webp"
]
pdf_path = compile_images(
    image_paths=custom_images,
    output_pdf="Report.pdf"
)
```

---

## 🧪 Running Tests

The test suite covers compilation, natural sorting, EXIF orientation, transparency handling, corrupted file handling, and CLI commands.

Run tests using `uv`:
```bash
uv run pytest -v
```

Or with standard `pytest`:
```bash
pytest -v
```

---

## 📁 Project Structure

```
pdf-compiler/
├── pyproject.toml         # Package metadata, scripts, and dependencies
├── uv.lock                # Locked dependency tree
├── README.md              # Documentation and guide
├── main.py                # Standalone entry point
├── src/
│   └── pdf_compiler/
│       ├── __init__.py    # Public library exports
│       ├── compiler.py    # Image processing and PDF creation logic
│       └── cli.py         # Command-line interface parser
└── tests/
    ├── test_compiler.py   # Unit tests for image compilation & sorting
    └── test_cli.py        # CLI argument parsing and execution tests
```
