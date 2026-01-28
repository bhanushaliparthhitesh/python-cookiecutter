# Python-Cookiecutter Codebase Understanding

This document provides a comprehensive understanding of the python-cookiecutter codebase to facilitate future development and maintenance.

## Overview

**python-cookiecutter** is a template generator that automatically creates a Python project structure ready for release on GitHub and PyPI. It's maintained by the Neuroinformatics Unit and provides a production-ready Python package boilerplate with all necessary tooling configured.

## How It Works

Users run the following command to generate a new Python project:
```bash
cookiecutter https://github.com/neuroinformatics-unit/python-cookiecutter
```

The tool prompts for project configuration (name, author, license, etc.) and generates a fully-configured Python package with best practices built-in.

## Repository Structure

```
python-cookiecutter/
├── .github/
│   └── workflows/
│       ├── test.yml                         # CI/CD for testing the cookiecutter
│       └── docs_build_and_deploy.yml        # Documentation deployment
├── {{cookiecutter.package_name}}/           # Template directory (the generated project)
│   ├── .github/workflows/                   # CI/CD workflows for generated projects
│   │   ├── test_and_deploy.yml             # Testing and PyPI deployment
│   │   └── docs_build_and_deploy.yml       # Sphinx docs deployment
│   ├── docs/                                # Sphinx documentation (optional)
│   ├── tests/                               # Test structure (unit & integration)
│   ├── {{cookiecutter.module_name}}/       # Python module directory
│   │   ├── __init__.py
│   │   ├── greetings.py                    # Example module (removed if no docs)
│   │   └── math.py                         # Example module (removed if no docs)
│   ├── .gitignore
│   ├── .pre-commit-config.yaml
│   ├── LICENSE
│   ├── MANIFEST.in
│   ├── pyproject.toml                      # Project configuration
│   └── README.md
├── hooks/
│   └── post_gen_project.py                  # Post-generation cleanup hook
├── tests/
│   └── test_cookiecutter.py                 # Tests for cookiecutter generation
├── cookiecutter.json                        # Cookiecutter configuration
├── pyproject.toml                           # Build configuration
├── .pre-commit-config.yaml                  # Pre-commit hooks
└── README.md
```

## Key Components

### 1. cookiecutter.json
The configuration file that defines interactive prompts for users:
- `full_name`: Author name
- `email`: Author email
- `github_username_or_organization`: GitHub username/org
- `package_name`: Package name (e.g., "my-package")
- `module_name`: Python module name (auto-generated from package_name)
- `short_description`: Package description
- `license`: License choice (BSD-3, MIT, Mozilla, Apache, LGPL, GPL)
- `create_docs`: Whether to include Sphinx documentation setup

### 2. Template Directory ({{cookiecutter.package_name}})
The actual project structure that gets generated. Uses Jinja2 template syntax:
- `{{cookiecutter.package_name}}` → User's package name
- `{{cookiecutter.module_name}}` → Python module name (normalized)

### 3. Post-Generation Hook (hooks/post_gen_project.py)
Runs after project generation to clean up based on user choices:
```python
if create_docs != "yes":
    shutil.rmtree("docs", ignore_errors=True)
    os.remove(".github/workflows/docs_build_and_deploy.yml")
    os.remove(f"{module_name}/greetings.py")
    os.remove(f"{module_name}/math.py")
shutil.rmtree("licenses", ignore_errors=True)
```

### 4. Testing Infrastructure
**Cookiecutter Tests** (`tests/test_cookiecutter.py`):
- `test_directory_names()`: Validates generated directory structure
- `test_docs()`: Ensures docs are conditionally created/removed
- `test_pyproject_toml()`: Validates all configuration values
- `test_pip_install()`: Tests pip installation of generated package

**Generated Project Tests**:
- pytest with coverage tracking
- Separate unit and integration test directories
- Pre-configured test runners (pytest, tox)

## Generated Project Features

### Tooling Included
1. **Version Management**: setuptools_scm (git-based versioning)
2. **Testing**: pytest, pytest-cov, tox (Python 3.11, 3.12, 3.13)
3. **Linting/Formatting**: 
   - Ruff (linting and formatting)
   - mypy (type checking)
   - pre-commit hooks
4. **Documentation**: Sphinx with PyData theme (optional)
5. **CI/CD**: GitHub Actions for testing, building, PyPI deployment

### Pre-commit Hooks (Generated Projects)
- `check-manifest`: Validates MANIFEST.in
- `check-added-large-files`: Prevents large files
- `check-toml`, `check-yaml`: Validates config files
- `end-of-file-fixer`, `trailing-whitespace`: Formatting
- `ruff`: Linting and formatting
- `codespell`: Spell checking

### GitHub Actions Workflows

**For the Cookiecutter Repository** (`.github/workflows/test.yml`):
- Runs on: push, PR, scheduled (weekly), manual dispatch
- **Linting**: Uses neuroinformatics-unit/actions/lint@v2
- **Testing**: Python 3.11-3.13 on Ubuntu, macOS, Windows
- **Coverage**: Reports to Codecov

**For Generated Projects** (`{{cookiecutter.package_name}}/.github/workflows/`):

1. **test_and_deploy.yml**:
   - Linting → Manifest check → Tests (multi-platform)
   - Build wheels on git tags
   - Upload to PyPI (if `TWINE_API_KEY` secret is set)

2. **docs_build_and_deploy.yml** (if docs enabled):
   - Builds Sphinx docs on PR/main push
   - Deploys to GitHub Pages on release tags

## Build, Test, and Lint

### For the Cookiecutter Repository
```bash
# Install dependencies
pip install pre-commit cookiecutter pyyaml pytest toml

# Run tests
pytest

# Run pre-commit hooks
pre-commit run --all-files
```

### For Generated Projects
```bash
# Install package with dev dependencies
pip install -e '.[dev]'

# Run tests with coverage
pytest

# Run linting
ruff check --fix
ruff format

# Type checking
mypy -p <module_name>

# Run all pre-commit hooks
pre-commit run --all-files

# Test multiple Python versions
tox
```

## Configuration Files

### pyproject.toml (Cookiecutter Repository)
- Build system: setuptools with setuptools-scm
- Ruff configuration: line-length=79, auto-fix enabled
- Codespell configuration

### pyproject.toml (Generated Projects)
Includes:
- Project metadata (name, authors, description, license)
- Dependencies and optional dependencies ([dev])
- Build system configuration
- Tool configurations (pytest, ruff, mypy, tox, codespell, setuptools)
- Python version requirement: >=3.11.0

## Development Workflow

### Making Changes to the Cookiecutter Template
1. Modify files in `{{cookiecutter.package_name}}/` directory
2. Update `hooks/post_gen_project.py` if cleanup logic changes
3. Update `tests/test_cookiecutter.py` to validate changes
4. Run tests: `pytest`
5. Test generation manually:
   ```bash
   cookiecutter . --no-input
   ```

### Testing Locally
```bash
# Generate a test project
cookiecutter /path/to/python-cookiecutter --no-input

# Or with custom config
cookiecutter /path/to/python-cookiecutter --config-file test_config.yaml --no-input
```

## Key Design Decisions

1. **Minimal Dependencies**: Only essential tools included
2. **Modern Python**: Requires Python 3.11+
3. **Git-based Versioning**: Uses setuptools_scm (no manual version management)
4. **Automated CI/CD**: GitHub Actions for testing and deployment
5. **Optional Documentation**: Users can opt-out of Sphinx setup
6. **License Flexibility**: Supports multiple open-source licenses
7. **Pre-commit Integration**: Enforces code quality from the start

## Common Use Cases

### Creating a New Python Package
```bash
# Install cookiecutter
pip install cookiecutter

# Generate project
cookiecutter https://github.com/neuroinformatics-unit/python-cookiecutter

# Follow prompts, then:
cd your-package-name
git init
git add .
git commit -m "Initial commit"
pip install -e '.[dev]'
pre-commit install
```

### Publishing to PyPI
Generated projects are configured for automated PyPI publishing:
1. Add `TWINE_API_KEY` secret to GitHub repository
2. Create a git tag: `git tag v0.1.0 && git push --tags`
3. GitHub Actions builds and publishes automatically

## Dependencies

### Cookiecutter Repository
- cookiecutter
- pytest
- pyyaml
- toml
- pre-commit

### Generated Projects (dev dependencies)
- pytest, pytest-cov, coverage
- tox
- mypy
- pre-commit
- ruff
- setuptools-scm

## Resources

- [Full Documentation](https://python-cookiecutter.neuroinformatics.dev)
- [Contribution Guidelines](https://python-cookiecutter.neuroinformatics.dev/contributing)
- [License: BSD-3-Clause](./LICENSE)
- [Project Chat](https://neuroinformatics.zulipchat.com/#narrow/channel/406003-Python-cookiecutter)

## Summary

The python-cookiecutter is a well-designed, production-ready template for Python packages. It automates the setup of testing, linting, documentation, CI/CD, and publishing infrastructure, allowing developers to focus on writing code rather than configuring tooling. The template follows modern Python best practices and includes comprehensive testing to ensure generated projects work correctly.
