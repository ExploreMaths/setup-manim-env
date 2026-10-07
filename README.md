# setup-manim-env

Reusable GitHub Action that installs Python plus everything Manim needs on `ubuntu-*` runners — extracted from [manim_extensions](https://github.com/ExploreMaths/manim_extensions) so any Manim repo can share one environment recipe.

## Usage

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: ExploreMaths/setup-manim-env@v1
        with:
          python-version: "3.14"
          install-full-deps: "true"   # TeX, ffmpeg, xvfb — required for rendering/tests
          install-editable: "true"    # pip install -e ".[dev]" in your repo
      - run: xvfb-run pytest
```

## Inputs

| Input               | Default   | Description                                                                 |
| ------------------- | --------- | --------------------------------------------------------------------------- |
| `python-version`    | `"3.14"`  | Python version to install                                                    |
| `install-full-deps` | `"false"` | Also install TeX Live (xetex + CJK), ffmpeg, xvfb, OpenGL/X11 headers, and Noto CJK / Amiri / JetBrains Mono fonts |
| `install-editable`  | `"false"` | Run `pip install -e ".[dev]"` in the working directory                       |

## Notes

- `opencv-python-headless` is always installed — Manim imports OpenCV and the headless build keeps GUI libraries out of CI.
- The full-deps set mirrors what Manim's own docs recommend for rendering and testing, including CJK and Arabic font support.
- `@v1` tracks the latest 1.x release; pin to a full semver tag if you want reproducibility.

## License

MIT
