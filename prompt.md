---
title: "Jekyll Migration Pipeline for GitHub Pages"
description: "CLI LLM Prompt to Migrate Static HTML + Markdown to Jekyll"
---

# Complete CLI LLM Prompt for Jekyll Migration

You are a senior DevOps engineer specializing in Jekyll and GitHub Pages migrations.
I will provide you with a **step-by-step pipeline** to restructure my existing static HTML + Markdown blog into a hybrid Jekyll setup.
Your task is to **generate the exact commands, file contents, and validation steps** for each part of the pipeline.

**Rules for Your Responses:**
1. Respond with **only the requested output** for each step (no explanations unless asked).
2. Use **absolute paths** where necessary.
3. Assume the working directory is the root of my GitHub Pages repo (`your-username.github.io`).
4. For file contents, provide the **complete file** with all necessary front matter.
5. Use **Bash commands** that work in a standard Unix environment.

---

## Pipeline Overview

We will proceed in this order:
1. Backup and Initialize
2. Directory Restructuring
3. Jekyll Configuration
4. Static HTML Integration
5. Markdown Migration
6. Asset Handling
7. Validation
8. Deployment

---

## Step 1: Backup and Initialize

**Task**: Backup the repo and set up Jekyll.
**Requirements**:
- Create a backup branch named `pre-jekyll-migration`
- Initialize Jekyll without overwriting existing files
- Install required gems

**Generate**:
1. Git commands to create and push a backup branch
2. Commands to initialize Jekyll with Bundler
3. Exact content for `Gemfile`
4. Command to install dependencies

---

## Step 2: Directory Restructuring

**Task**: Reorganize files/folders according to the new structure.
**Requirements**:
- Move `css/styles.css` → `assets/css/styles.css`
- Move `images/` → `assets/images/`
- Create new directories: `_posts/`, `_notes/`, `_includes/`, `_layouts/`, `_sass/`, `assets/js/`
- Preserve all existing files in `notes/`

**Generate**:
1. Bash commands to create all new directories
2. Bash commands to move files (use `-i` flag to prevent overwrites)
3. Verification commands to ensure no files were lost

---

## Step 3: Jekyll Configuration

**Task**: Create `_config.yml` and layout files.
**Requirements**:
- Hybrid structure supporting both static HTML and Markdown
- MathJax support via kramdown
- Include static files: `cv.html`, `publications.html`, `index.html`
- Collections for notes

**Generate**:
1. Complete content for `_config.yml`
2. Complete content for these files:
   - `_layouts/default.html`
   - `_layouts/static.html`
   - `_includes/head.html` (with MathJax CDN)
   - `_includes/header.html`
   - `_includes/footer.html`
3. Bash commands to create these files

---

## Step 4: Static HTML Integration

**Task**: Add Jekyll front matter to static HTML files.
**Requirements**:
- Add front matter to: `cv.html`, `publications.html`, `index.html`
- Use `layout: static` for all static files
- Preserve all existing HTML content

**Generate**:
1. Exact front matter to prepend to each file
2. Bash commands to add front matter (use `sed` or `awk`)
3. Verification command to check front matter was added

---

## Step 5: Markdown Migration

**Task**: Migrate notes to Jekyll format.
**Requirements**:
- Convert `notes/post1_ito_strato/index.md` → `_posts/2026-05-03-ito-stratonovich.md`
- Update image paths to `/assets/images/notes/ito_strato/...`
- Add proper front matter with categories and tags
- Preserve all LaTeX content
- Handle Python code references

**Generate**:
1. Complete content for `_posts/2026-05-03-ito-stratonovich.md`
2. Bash commands to:
   - Copy and rename the Markdown file
   - Move assets to `assets/images/notes/ito_strato/`
   - Move Python scripts to `scripts/ito_strato/`
3. Repeat for `post2_kolmogorov_equation`

---

## Step 6: Asset Handling

**Task**: Update all asset references.
**Requirements**:
- Replace `css/styles.css` → `/assets/css/styles.css`
- Replace `images/xxx.png` → `/assets/images/xxx.png`
- Update references in both HTML and Markdown files

**Generate**:
1. `sed` commands to update paths in:
   - All `.html` files
   - All `.md` files in `_posts/` and `_notes/`
2. Verification commands to check for remaining old paths
3. Command to find all image references

---

## Step 7: Validation

**Task**: Test the Jekyll site locally.
**Requirements**:
- Run local server
- Check for 404 errors
- Validate key pages:
  - `/cv`
  - `/publications`
  - `/notes/ito-stratonovich`
  - All asset paths

**Generate**:
1. Command to run Jekyll locally
2. Command to install and run htmlproofer for link checking
3. Bash commands to check for:
   - Broken links
   - Missing assets
   - 404 errors in terminal output

---

## Step 8: Deployment

**Task**: Push changes to GitHub.
**Requirements**:
- Commit all changes with descriptive message
- Push to main branch
- Verify GitHub Pages builds successfully

**Generate**:
1. Git commands to:
   - Add all changes
   - Commit with message: "Migrate to Jekyll hybrid structure"
   - Push to main branch
2. Steps to verify GitHub Pages build status
3. Command to check the live site

---

## Execution Instructions

1. Copy this entire prompt to your LLM
2. Request Step 1 output with: "Generate output for Step 1"
3. After executing Step 1 commands, request Step 2 with: "Proceed to Step 2"
4. Continue until all steps are completed
5. For any errors, provide the error output and ask for corrected commands

---

## Verification Checklist

After completion, verify:
- [ ] All static HTML pages render correctly
- [ ] Markdown posts display properly with MathJax
- [ ] All images and assets load
- [ ] Navigation between pages works
- [ ] No 404 errors in build output
- [ ] GitHub Pages builds successfully
```

To use this:
1. Copy the entire content above
2. Paste into a file named `jekyll_migration_prompt.md`
3. Feed it to your CLI LLM one step at a time

The prompt is designed to:
- Be completely self-contained
- Handle both static HTML and Markdown content
- Preserve all existing assets
- Include proper error checking
- Maintain your existing URLs where possible