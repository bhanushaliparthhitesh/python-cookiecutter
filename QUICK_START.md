# python-cookiecutter - Quick Reference

## What is it?

**python-cookiecutter** is a **project template generator** that creates production-ready Python packages in seconds.

## Purpose (One Line)

Automates the setup of a professional Python project with all essential tooling pre-configured, so you can start coding immediately.

## What Problem Does It Solve?

Instead of spending hours setting up:
- ✅ Project structure and packaging
- ✅ Testing framework (pytest, tox)
- ✅ Code quality tools (ruff, mypy, pre-commit hooks)
- ✅ Documentation (Sphinx)
- ✅ CI/CD pipelines (GitHub Actions)
- ✅ PyPI publishing workflow
- ✅ Version management (setuptools_scm)

**You get all of this in 30 seconds.**

## How It Works

```bash
# 1. Install cookiecutter
pip install cookiecutter

# 2. Generate your project
cookiecutter https://github.com/neuroinformatics-unit/python-cookiecutter

# 3. Answer a few questions (name, license, etc.)
# 4. Start coding!
```

## Who Should Use It?

- Python developers starting a new package
- Teams wanting consistent project structure
- Anyone publishing to PyPI
- Projects requiring professional tooling from day one

## What You Get

A complete Python package with:
- **Automated testing** on every commit
- **Code formatting** and quality checks
- **Documentation** that auto-deploys to GitHub Pages
- **One-command publishing** to PyPI via git tags
- **Professional structure** following best practices

## Example Output

After running cookiecutter, you'll have:
```
my-awesome-package/
├── my_awesome_package/      # Your Python module
├── tests/                   # Test structure
├── docs/                    # Sphinx documentation
├── .github/workflows/       # CI/CD automation
├── pyproject.toml           # Package configuration
└── All tooling configured!
```

## Key Benefit

**Go from zero to a production-ready Python package in under a minute.**

---

For detailed documentation, see [CODEBASE_UNDERSTANDING.md](./CODEBASE_UNDERSTANDING.md)
