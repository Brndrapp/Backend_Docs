# Maintaining the docs site

Notes for whoever publishes and updates this Swagger UI site. Reader-facing
info is in `README.md`.

## Contents

- `index.html`    — Swagger UI page (loads UI from unpkg CDN, points at `./swagger.json`)
- `swagger.json`  — OpenAPI 2.0 spec (generated from the backend's `@`-annotations)
- `.nojekyll`      — tells GitHub Pages to serve files verbatim (skip Jekyll)
- `README.md`      — reader-facing landing page

## Deploying to GitHub Pages

This folder is its **own git repository** (`git@github.com:Brndrapp/Backend_Docs.git`),
separate from the `brndr` monorepo. The `origin` remote is already configured.

To publish changes, from inside this folder:

```bash
git push origin main
```

Then on GitHub: **repo → Settings → Pages → Source = Deploy from a branch →
`main` / root**. Site goes live at `https://brndrapp.github.io/Backend_Docs/`.

## Updating the spec

`swagger.json` is generated from `@`-annotations in the backend's `main.go`
via `swag`. After editing routes in the backend, regenerate and copy it in:

```bash
cd ../                           # back to 01_backend
swag init -g main.go             # regenerates docs/swagger.json
cp docs/swagger.json docs_site/swagger.json
cd docs_site
git commit -am "docs: refresh swagger.json"
git push
```

> **Note:** the copied spec can drift from `main.go` if routes change without a
> regenerate. Regenerate before pushing if the backend has moved.
