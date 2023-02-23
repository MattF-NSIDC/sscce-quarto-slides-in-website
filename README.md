## Problems

I created this project with the following steps:

1. Create project template
    ```
    quarto create-project sscce-quarto-slides-in-website --type website
    ```
1. Create `slides/index.qmd` with `format: 'revealjs'` in front-matter.
1. Add link from `index.qmd` to `slides/index.qmd`
1. `quarto preview index.qmd`


### Preview is not idempotent - slides render as plain HTML on first preview

_Confirmed as bug: <https://github.com/quarto-dev/quarto-cli/issues/4469>_

In the `main` branch, slides don't consistently render as slides when previewing.
`quarto render` is _not_ affected by this issue.

* On running `quarto preview index.qmd`, slides render as a regular HTML page using
  "cosmo" theme.

* Press CTRL+C to quit `quarto preview`, then run `quarto preview index.qmd` again. Now,
  the slides render as a RevealJS presentation.

* Delete `_site` and `.quarto` and run the preview again, and the slides will again show
  as a HTML page with "cosmo" theme.

---

Discovered with aid of a Quarto contributor that `quarto preview` behaves better on
first run than `quarto preview [file]`. However, after triggering a hot-reload
(depending on which file was edited to trigger the hot-reload), the behavior changes.
Please see the linked bug report for full details.


### On adding `revealjs` config to `_quarto.yml`, render fails

_Confirmed as user error: <https://github.com/quarto-dev/quarto-cli/issues/4470>_

Render and preview both fail with:

```
ERROR: NotFound: No such file or directory (os error 2), rename
'/path/to/quarto/project/index.html' -> '/path/to/quarto/project/_site/index.html'
```

* Add a `format.revealjs` block with `toc: true` to `_quarto.yml`.

* Run `quarto render` or `quarto preview index.qmd`, and observe the error message.

You can also check out the `revealjs-config-breaks-preview` branch to reproduce.


#### Solution: User error!

Thanks to @cscheid in <https://github.com/quarto-dev/quarto-cli/issues/4470>, I now
understand that specifying both `html` and `revealjs` in `_quarto.yml` causes all markdown
to be rendered with both forms, and they're both HTML-type outputs, so the output files
conflict. The solution is to use `_metadata.yml` to specify `revealjs` settings inside
the `slides` directory.
