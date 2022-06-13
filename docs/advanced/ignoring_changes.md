You may find that your screenshots are changing every time you run rich-codex, even though no relevant changes have occured within your code. This could be because the screenshots include timestamps or some other live data.

To avoid doubling your commit count with changes that you don't care about, rich-codex has two mechanisms which you can use to ignore changes:

- ⚖️ Percentage change in file contents
- 🔎 Regular expression matches

## Percentage change in file contents

When you run rich-codex, any new images created will generate log messages that look like this:
`Saved: 'docs/img/rich-codex-snippet-title.svg' (4.63% change)`.
This percentage change is calculated using the [python-Levenshtein](https://github.com/ztane/python-Levenshtein) package, comparing the raw bytes of the two files.

By default, any new files with 0.00% change will be ignored. If you find that you have screenshots changing by the same small percentage every time, you can raise this threshold by setting `--min-pct-diff` (cli) / `MIN_PCT_DIFF` (env var / markdown comment) / `min_pct_diff` (GitHub action input).

For example, if a timestamp caused this file to change by 4.34% on every commit, those changes could be ignored as follows:

```markdown
<!-- RICH-CODEX MIN_PCT_DIFF=5 -->

![`rich-codex --help`](../img/rich-codex-help.svg)
```

## Regular expression matches

Percentage changes in files is quick and simple, but a little crude. If you prefer, you may be able to use regular expressions instead with `--skip-change-regex` (cli) / `SKIP_CHANGE_REGEX` (env var / markdown comment) / `skip_change_regex` (GitHub action input).

If there is a > 0% change in files, a rich diff will be generated. Any diff lines matching the supplied regexes will be removed and if none remain, the changeset will be ignored.

Rich-codex ships with one default, applied for PDF files: if the only change is a line with `"/CreationDate"` then the changeset will be ignored.

> ⏱ Please note that generating diffs between file pairs can be very slow. Use with caution.
