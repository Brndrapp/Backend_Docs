# Brndr API Docs (Swagger UI)

Static Swagger UI site for the Brndr backend API. Self-contained: just
`index.html` (loads Swagger UI from a CDN) + `swagger.json` (the OpenAPI spec).

This folder is its **own git repository** so it can be pushed to a standalone
GitHub repo and served via GitHub Pages.

## Contents

- `index.html`     — Swagger UI page (loads UI from unpkg, points at `./swagger.json`)
- `swagger.json`   — OpenAPI 2.0 spec (copied from `../docs/swagger.json`)
- `.nojekyll`       — tells GitHub Pages to serve files verbatim (skip Jekyll)

## Deploying to GitHub Pages

From inside this folder:

```bash
git remote add origin git@github.com:petrobsel/<repo-name>.git   # e.g. brndr-docs
git push -u origin main
```

Then on GitHub: **repo → Settings → Pages → Source = Deploy from a branch →
`main` / root**. Site goes live at:

```
https://petrobsel.github.io/<repo-name>/
```

## Updating the spec

`swagger.json` is generated from `@`-annotations in `../main.go` via `swag`.
After editing routes in the backend, regenerate and copy it in:

```bash
cd ..                                  # back to 01_backend
swag init -g main.go                    # regenerates docs/swagger.json
cp docs/swagger.json docs-site/swagger.json
cd docs-site
git commit -am "docs: refresh swagger.json"
git push
```

## Note on "Try it out"

The spec declares `host: localhost:8080`, so the in-page "Try it out" button
targets your local backend by default. Point it at a deployed server by changing
the server/host field in Swagger UI, or by editing `host` in `swagger.json`.
