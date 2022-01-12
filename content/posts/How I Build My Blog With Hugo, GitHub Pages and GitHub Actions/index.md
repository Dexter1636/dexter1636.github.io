+++
title = "How I Build My Blog With Hugo, GitHub Pages and GitHub Actions"
description = ""
math = false
type = ["posts","post"]
tags = [
    "development",
    "personal website"
]
date = 2022-01-12
categories = [
    "Development",
]
series = []
[ author ]
  name = "Dexter"

+++

This blog is about how I build my blog. The blog is built with Hugo and hosted in GitHub Pages.

If you are interested in building your own site using Hugo and GitHub Pages, focus on the `Quick Start` part. Follow the given references and you can make it.

Other parts are about my solutions to some problems or special requirements I faced when I built it.

### Quick Start

Just follow [Hugo - Quick Start](https://gohugo.io/getting-started/quick-start/) and [Hugo - Host on GitHub](https://gohugo.io/hosting-and-deployment/hosting-on-github/).

Also, there is a detailed blog that you may refer: [Build a Personal Website With GitHub Pages and Hugo](https://levelup.gitconnected.com/build-a-personal-website-with-github-pages-and-hugo-6c68592204c7).

### Content Organization

In my blog, it is like

```
content/
├── about
│   ├── index.md
├── posts
│   ├── my-post
│   │   ├── content1.md
│   │   ├── content2.md
│   │   ├── image1.jpg
│   │   ├── image2.png
│   │   └── index.md
│   └── my-other-post
│       └── index.md
│
└── another-section
    ├── ..
    └── not-a-leaf-bundle
        ├── ..
        └── another-leaf-bundle
            └── index.md
```

See [Content Organization](https://gohugo.io/content-management/organization/) and [examples-of-leaf-bundle-organization](https://gohugo.io/content-management/page-bundles/#examples-of-leaf-bundle-organization) for detail.

### Enable MathJax

[MathJax](https://www.mathjax.org/) is a JavaScript display engine for mathematics that works in all browsers.

To enable MathJax, add the following code:

```toml
{{ if .Params.math }}
<script>
  MathJax = {
    tex: {
      inlineMath: [["$", "$"]],
    },
    displayMath: [
      ["$$", "$$"],
      ["\[\[", "\]\]"],
    ],
    svg: {
      fontCache: "global",
    },
  };
</script>
<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script
  id="MathJax-script"
  async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"
></script>
{{ end }}
```

Then, in the `Front Matter` of your post:

```
+++
...
math = true
+++
```

### Support for Raw HTML

If you use raw html in your markdown file, write these to your config file:

```toml
[markup.goldmark.renderer]
unsafe=true
```

### Deploy Using GitHub Actions

[GitHub Actions](https://github.com/features/actions) makes it easy to build, test, and deploy your code right from GitHub, so that every time you push your code on the Github repository, GitHub Actions will build the site automatically.

Create a file in `.github/workflows/gh-pages.yml` containing the following content:

```yml
name: github pages

on:
  push:
    branches:
      - master  # Set a branch to deploy
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-20.04
    steps:
      - name: Checkout
        uses: actions/checkout@v2
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          extended: true

      - name: Clean public directory
        run: rm -rf public

      - name: Build
        run: hugo --minify

      # - name: Create cname file  # only needed when you use a custom domain
      #   run: echo 'freshswift.net' > public/CNAME

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/master'
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

After that, the GitHub action will push the generated content to the `gh-pages` branch once you push your code. This means GitHub has to serve your `gh-pages` branch as a GitHub Pages branch. You can change this setting by going to Settings > GitHub Pages, and change the source branch to `gh-pages`.

### Reference

- [Hugo](https://gohugo.io/)
- [Build a Personal Website With Github Pages and Hugo](https://levelup.gitconnected.com/build-a-personal-website-with-github-pages-and-hugo-6c68592204c7)
- [在Hugo中使用MathJax](https://note.qidong.name/2018/03/hugo-mathjax/)
- [A Github Actions Workflow to Generate & Publish your Hugo Website](https://medium.com/zendesk-engineering/a-github-actions-workflow-to-generate-publish-your-hugo-website-f36375e56cf7)
