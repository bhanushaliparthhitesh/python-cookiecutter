# Documentation Configuration Files - Understanding Guide

This document provides a comprehensive understanding of the `conf.py` and `custom.css` files in the python-cookiecutter repository's documentation system.

## Files Overview

The repository contains documentation configuration in two locations:

1. **Root Documentation** (`docs/source/`) - For the cookiecutter template project itself
2. **Template Documentation** (`{{cookiecutter.package_name}}/docs/source/`) - Template that gets copied to generated projects

---

## 1. Root conf.py (`docs/source/conf.py`)

**Location:** `/docs/source/conf.py`  
**Purpose:** Sphinx configuration for the python-cookiecutter documentation website

### Key Configuration Sections

#### Project Metadata (Lines 6-17)
```python
project = "python-cookiecutter"
copyright = "2025, University College London"
author = "Neuroinformatics Unit"
```
- Defines project name, copyright, and author
- Uses `importlib.metadata` to auto-detect version from installed package
- Falls back to "0.0.0" if git is not initialized (allows local builds)

#### Sphinx Extensions (Lines 20-32)
```python
extensions = [
    "myst_parser",           # Markdown support
    "sphinx_design",         # Design components (cards, grids)
    "sphinx.ext.autodoc",    # Auto-generate docs from docstrings
    "sphinx.ext.napoleon",   # Google/NumPy style docstrings
    "sphinx.ext.githubpages", # GitHub Pages support
    "sphinx_autodoc_typehints", # Type hint documentation
    "sphinx.ext.autosummary",  # Auto-summary tables
    "sphinx.ext.viewcode",   # Links to source code
    "sphinx.ext.intersphinx", # Cross-project links
    "sphinx_sitemap",        # Generate sitemap.xml
    "nbsphinx",              # Jupyter notebook support
]
```

#### MyST Parser Configuration (Lines 34-54)
- Enables advanced Markdown features:
  - `colon_fence`: Fenced code blocks with colons
  - `deflist`: Definition lists
  - `html_admonition`: HTML-style admonitions
  - `linkify`: Auto-convert URLs to links
  - `tasklist`: GitHub-style task lists
  - `attrs_inline`: Inline attributes
- Sets heading anchor depth to 4 levels
- Supports both `.md` (Markdown) and `.rst` (reStructuredText) files

#### HTML Theme Configuration (Lines 63-93)
```python
html_theme = 'pydata_sphinx_theme'
html_logo = "_static/dark-logo-niu.png"
html_title = "Python Cookiecutter"
html_favicon = "_static/favicon.ico"
```

**Theme Options:**
- Uses PyData Sphinx Theme (same theme as Pandas, NumPy, SciPy)
- **Navbar Configuration:**
  - `navbar_start`: Logo and navigation items (left-aligned)
  - Icon links to GitHub and Zulip chat
- **Logo:** Displays project name + version
- **Footer:** Custom footer sections

#### Sitemap Configuration (Lines 95-98)
```python
github_user = "neuroinformatics-unit"
html_baseurl = "https://python-cookiecutter.neuroinformatics.dev"
```
- Generates sitemap for SEO
- Custom domain configuration

#### Sidebar Configuration (Lines 101-107)
```python
html_sidebars = {
    "**": ["sidebar-nav.html"],  # All pages have sidebar
    "index": []                   # Index page has NO sidebar
}
```
- Navigation sidebar on all pages except homepage
- Homepage gets full-width layout

#### Custom CSS (Lines 110-112)
```python
def setup(app):
    app.add_css_file("css/custom.css")
```
**Critical:** This function adds the custom.css file to all documentation pages

#### 404 Page Configuration (Lines 114-130)
- Custom 404 error page with helpful message
- No URL prefix (for GitHub Pages deployment)

#### Link Checking (Lines 132-143)
- Ignores certain URLs and anchors during link validation
- Prevents false positives for Zulip and GitHub URLs

---

## 2. Custom CSS (`docs/source/_static/css/custom.css`)

**Location:** `/docs/source/_static/css/custom.css`  
**Purpose:** Custom styling for the documentation website

### Styling Sections

#### 1. Theme Colors (Lines 1-9)
```css
html[data-theme=dark] {
    --pst-color-primary: #04B46D;  /* Green for dark mode */
    --pst-color-link: var(--pst-color-primary);
}

html[data-theme=light] {
    --pst-color-primary: #03A062;  /* Darker green for light mode */
    --pst-color-link: var(--pst-color-primary);
}
```
**Purpose:** Brand colors (Neuroinformatics Unit green) for both light and dark themes

#### 2. Article Container Width (Lines 11-13)
```css
body .bd-article-container {
    max-width: 100em !important;
}
```
**Purpose:** Increases maximum width of article content area

#### 3. Navbar Logo Fix (Lines 14-35)
```css
.navbar-header-items__start .navbar-item { width: 100%; }
.navbar-item .navbar-brand { width: 100%; }
.navbar-brand img { 
    min-width: 0; 
    height: auto; 
    max-height: 100%; 
    flex-shrink: 1; 
}
```
**Purpose:** Prevents logo from being too wide and clashing with navbar items  
**Reference:** Fix from PyData theme issue #1143

#### 4. Sponsors Section (Lines 37-56)
```css
.col { flex: 0 0 50%; max-width: 50%; }
.img-sponsor { height: 50px; padding: 5px; }
.things-in-a-row { display: flex; flex-wrap: wrap; }
```
**Purpose:** Layout for sponsor logos (if any)

#### 5. Card Styling (Lines 58-72)
```css
.sd-card-icon { 
    color: var(--sd-color-primary); 
    font-size: 1.5em; 
}
.sd-card { padding: 1.5rem; transition: transform 0.2s; }
.sd-card:hover { transform: translateY(-5px); }
```
**Purpose:** 
- Matches card icon colors to theme
- Adds hover animation (cards lift up on hover)

#### 6. Full-Width Content (Lines 74-88)
```css
.bd-page-width { max-width: 90% !important; }
body[data-hide-sidebar="true"] .bd-sidebar-primary { display: none !important; }
body[data-hide-sidebar="true"] .bd-main { flex-grow: 1; max-width: 75%; }
```
**Purpose:**
- Increases page width to 90% of viewport
- Supports hiding sidebar with metadata tag
- Expands content when sidebar is hidden

---

## 3. Template conf.py (`{{cookiecutter.package_name}}/docs/source/conf.py`)

**Location:** `{{cookiecutter.package_name}}/docs/source/conf.py`  
**Purpose:** Sphinx configuration template for generated projects

### Key Differences from Root conf.py

#### 1. Project Information (Lines 23-33)
```python
project = "{{cookiecutter.package_name}}"
copyright = "2022, {{cookiecutter.full_name}}"
author = "{{cookiecutter.full_name}}"
```
**Uses Jinja2 templating** - Values are filled in when user generates a project

#### 2. Simpler Extensions List (Lines 38-49)
- Does NOT include `sphinx_design` extension
- Maintains core functionality only
- More minimal setup for generated projects

#### 3. MyST Parser Extensions (Lines 52-67)
**Additional extensions enabled:**
- `amsmath`: Advanced math typesetting
- `dollarmath`: Math with $ delimiters
- `strikethrough`: ~~strikethrough text~~

#### 4. Simpler Theme Options (Lines 94-111)
- Only GitHub icon link (no Zulip)
- Simpler logo configuration
- No footer customization

#### 5. Custom CSS Support (Lines 121-128)
```python
html_static_path = ['_static']

def setup(app):
    app.add_css_file("css/custom.css")
```
**NEW:** Template now includes custom CSS support with:
- Brand colors (green theme)
- Navbar header spacing adjustments
- Responsive navbar styling

**Custom CSS File:** `{{cookiecutter.package_name}}/docs/source/_static/css/custom.css`

#### 6. Dynamic URLs (Lines 117-119)
```python
github_user = "{{cookiecutter.github_username_or_organization}}"
html_baseurl = f"https://{github_user}.github.io/{project}"
```
**Uses template variables** for GitHub Pages URL generation

---

## 4. Template Custom CSS (`{{cookiecutter.package_name}}/docs/source/_static/css/custom.css`)

**Location:** `{{cookiecutter.package_name}}/docs/source/_static/css/custom.css`  
**Purpose:** Custom styling for generated project documentation

### Styling Sections

#### 1. Theme Colors
```css
html[data-theme=dark] {
    --pst-color-primary: #04B46D;  /* Bright green for dark mode */
    --pst-color-link: var(--pst-color-primary);
}

html[data-theme=light] {
    --pst-color-primary: #03A062;  /* Darker green for light mode */
    --pst-color-link: var(--pst-color-primary);
}
```
**Purpose:** Neuroinformatics Unit brand colors for both themes

#### 2. Navbar Header Spacing
```css
.bd-header .navbar-header-items__start {
  margin-right: 0 !important;
  padding-right: 1.5rem;
}
```
**Purpose:** Controls spacing between navbar logo/nav items and right-side elements

#### 3. Navbar Item Styling
```css
.navbar-header-items__start .navbar-item { width: 100%; }
.navbar-item .navbar-brand { width: 100%; }
.navbar-brand img { 
    min-width: 0; 
    height: auto; 
    max-height: 100%; 
    flex-shrink: 1; 
}
```
**Purpose:** Responsive navbar layout preventing logo overflow

---

## Key Relationships

### conf.py → custom.css Connection
**Root Documentation:**
```python
# In docs/source/conf.py (line 110-112)
def setup(app):
    app.add_css_file("css/custom.css")
```

**Template Documentation:**
```python
# In {{cookiecutter.package_name}}/docs/source/conf.py (lines 127-128)
def setup(app):
    app.add_css_file("css/custom.css")
```

This is the **critical link** - conf.py loads custom.css into every documentation page.

### File Paths
```
docs/
└── source/
    ├── conf.py              # Sphinx configuration (loads custom.css)
    └── _static/
        ├── css/
        │   └── custom.css   # Custom styles for cookiecutter docs
        ├── dark-logo-niu.png
        ├── light-logo-niu.png
        └── favicon.ico

{{cookiecutter.package_name}}/
└── docs/source/
    ├── conf.py              # Template Sphinx config (loads custom.css)
    └── _static/
        └── css/
            └── custom.css   # Custom styles for generated projects
        └── favicon.ico
```

---

## Common Customization Tasks

### Changing Brand Colors
**File:** `custom.css` (lines 1-9)
```css
--pst-color-primary: #04B46D;  /* Change this hex color */
```

### Modifying Page Width
**File:** `custom.css` (lines 11-13, 75-76)
```css
max-width: 100em !important;  /* Adjust this value */
```

### Adding/Removing Sphinx Extensions
**File:** `conf.py` (lines 20-32)
```python
extensions = [
    "new_extension_name",  # Add new extension here
]
```

### Changing Theme
**File:** `conf.py` (line 63)
```python
html_theme = 'pydata_sphinx_theme'  # Change to different theme
```

### Adjusting Navbar Icons
**File:** `conf.py` (lines 73-86)
```python
"icon_links": [
    {
        "name": "New Link",
        "url": "https://example.com",
        "icon": "fa-brands fa-icon-name",
        "type": "fontawesome",
    }
]
```

---

## Build Commands

### Local Documentation Build
```bash
cd docs
pip install -r requirements.txt  # Install dependencies
make html                        # Build HTML documentation
# Output: docs/build/html/index.html
```

### Clean Build
```bash
cd docs
make clean  # Remove old builds
make html   # Rebuild from scratch
```

---

## Summary

**Root conf.py (docs/source/conf.py):**
- Comprehensive configuration for cookiecutter's own documentation
- Uses PyData Sphinx Theme with custom branding
- Loads custom.css for styling
- Includes many Sphinx extensions for rich documentation

**Custom CSS (docs/source/_static/css/custom.css):**
- Brand colors (green theme for Neuroinformatics Unit)
- Navbar logo fixes
- Card hover animations
- Full-width content support
- Sidebar hiding capability

**Template conf.py ({{cookiecutter.package_name}}/docs/source/conf.py):**
- Minimal template for generated projects
- Uses Jinja2 variables for customization
- NO custom CSS by default
- Simpler extension set

Both files work together to create the professional, branded documentation website visible at https://python-cookiecutter.neuroinformatics.dev
