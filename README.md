# tc39-minisite-example

An example repo showing a technique for building a TC39 mini-site for proposals. This is based on the mini-site used in [Types for Comments proposal you can see here](https://giltayar.github.io/proposal-types-as-comments/). You can read the technical reasoning for how the components are built in [REASONING.md](./REASONING.md).

### How to use it

Copy over the `site` directory from this repo to your proposal repo. Then run `npm i` and `npm start` to load up the dev server. Editing [`./site/src/site.jsx`](./site/src/site.jsx) will live update the development server as you save the file. The `site.jsx` uses a series of components in order to write at a higher level abstraction in that file.

### Deploy to GitHub

To have the site automatically updated on every commit, add `.github/workflows/Deploy.yml`:

```yml
# Deploys the minisite in the ./site folder to 
# the GitHub pages branch on this repo.

name: Update GitHub Pages

on:
  push:
    branches:
      - master
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: '16'

      - name: Build Site
        run: |
          cd site
          npm install
          npm run build
          cp -r assets out

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./site/out

```

Then go to your repo settings, GitHub pages settings and set the "Source" to `gh-pages` and "/". Then it should be available at the URL in the site when the `yml` file above is committed to the main or master branch.