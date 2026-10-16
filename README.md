# Nan Li's academic website

Personal website: <https://nanlimarketing.github.io>

This site uses the al-folio v1 starter and versioned plugin gems. The migration
starts from upstream commit `ba1c8104a2f39c76ebc2fa9da1b02c66b75ea855` and preserves
the site's existing personal content and public routes.

## Editing content

| Content                                            | Source                             |
| -------------------------------------------------- | ---------------------------------- |
| Biography, profile photo reference, office address | `_pages/about.md`                  |
| Research page and work in progress                 | `_pages/publications.md`           |
| Accepted and published papers                      | `_bibliography/papers.bib`         |
| Working papers                                     | `_bibliography/working_papers.bib` |
| Teaching                                           | `_pages/teaching.md`               |
| CV data                                            | `assets/json/resume.json`          |
| Downloadable CV                                    | `assets/pdf/CV.pdf`                |
| Contact and academic profiles                      | `_data/socials.yml`                |
| Coauthor links                                     | `_data/coauthors.yml`              |
| Site settings and feature flags                    | `_config.yml`                      |

The public routes remain `/`, `/research/`, `/cv/`, and `/teaching/`.
Existing sample posts, news, projects, and their attachments remain available.
The blog is kept out of the main navigation, as before.

## Build and preview

Use Ruby 3.3.5 (the CI version), Node.js, ImageMagick, and Jupyter nbconvert.

```sh
gem install bundler -v 4.0.6
bundle install
npm ci
bundle exec al-folio upgrade audit
bundle exec al-folio upgrade overrides audit --fail-on-stale
bundle exec jekyll build
bundle exec jekyll serve
```

The local site is served at <http://localhost:4000/>. This is a user site at the
domain root, so keep `baseurl: ""`. The upstream demo's `/al-folio` base URL does
not apply here.

## Deployment and updates

The deployment workflow builds pushes to `master` or `main` and publishes `_site`
to `gh-pages`. Pull requests build without publishing. Before pushing, confirm
that `origin` points to `nanlimarketing/nanlimarketing.github.io`.

For future theme updates, review the owning plugins' release notes, update their
exact version pins in `Gemfile`, refresh `Gemfile.lock`, and repeat the build and
upgrade checks. Keep the `Gemfile` and `_config.yml` plugin lists consistent.

Do not copy old theme internals over gem-owned files. If a future change requires
a local override, review it and record it using `al-folio upgrade overrides`.

See [the migration record](docs/MIGRATION.md) for the preservation checks,
compatibility choices, and rollback instructions. General upstream documentation
is available in [docs/README.md](docs/README.md).
