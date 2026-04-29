# Hugo NGUYEN - Academic Website

Personal academic website for Hugo NGUYEN, PhD Student at École Polytechnique (CMAP) and Quantitative Researcher at Crédit Agricole SA.

## 📄 License

© 2026 Hugo NGUYEN. All rights reserved.

## Jekyll Setup

This site uses [Jekyll](https://jekyllrb.com/) for static site generation. Blog posts go in the `_posts/` directory as markdown files, named like `YYYY-MM-DD-title.md`.

### Local Development

1. Install Jekyll (requires Ruby):
   ```sh
   gem install bundler jekyll
   ```
2. Serve the site locally:
   ```sh
   bundle exec jekyll serve
   # or, if you don't use Bundler:
   jekyll serve
   ```
3. Visit `http://localhost:4000` in your browser.

### Deployment

Push to your repository. If using GitHub Pages, set the source to the root or `/docs` folder as needed.

---

- Edit `_config.yml` to change site settings.
- Add posts in `_posts/` with YAML front matter.
- Customize layouts in `_layouts/` (optional).
