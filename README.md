## Problems


### Preview is not idempotent - slides render as plain HTML on first preview

In the current state, slides don't consistently render as slides when previewing.

* On running `quarto preview index.qmd`, slides render as a regular HTML page using
  "cosmo" theme.

* Press CTRL+C to quit `quarto preview`, then run `quarto preview index.qmd` again. Now,
  the slides render as a RevealJS presentation.

* Delete `_site` and `.quarto` and run the preview again, and the slides will again show
  as a HTML page with "cosmo" theme.


### On adding `revealjs` config to `_quarto.yml`, preview fails

Preview fails with:

```
ERROR: NotFound: No such file or directory (os error 2), rename
'/path/to/quarto/project/index.html' -> '/path/to/quarto/project/_site/index.html'
```

* Add a `format.revealjs` block with `toc: true` to `_quarto.yml`.

* Run `quarto preview index.qmd`, and the preview will fail to load.


## Notes

* Is this only true for `preview`, or also `render`?
