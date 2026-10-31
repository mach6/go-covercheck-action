# go-covercheck-action

GitHub Action that runs [go-covercheck](https://github.com/mach6/go-covercheck) to enforce Go test coverage thresholds.

```yaml
steps:
  - uses: actions/checkout@v7
  - uses: actions/setup-go@v7
    with:
      go-version-file: go.mod
  - run: go test -coverprofile=coverage.out ./...
  - uses: mach6/go-covercheck-action@v1
    with:
      coverage-file: coverage.out
      statement-threshold: "80"
      block-threshold: "70"
      comment: ${{ github.event_name == 'pull_request' }}
```

## How it works

- **Linux runners:** runs the prebuilt `ghcr.io/mach6/go-covercheck:<version>` image. Nothing is compiled.
- **macOS, Windows, or Linux without Docker:** builds that release with `go install` (needs `actions/setup-go`).

The CLI release is chosen by the `version` input, which defaults to a released version (currently `0.6.1`).
Inputs for newer CLI features (line and function thresholds, `comment`, `diff-from`, `table-style`, `no-uncovered-*`) need a CLI release that has them, so set `version` accordingly.
Inputs are passed to the script as environment variables, never interpolated into the shell.

`comment: true` posts the results on the pull request and needs `pull-requests: write` permission.
`skip` takes newline or comma separated regexes.

## Inputs

| Input | Description | Default |
|-------|-------------|---------|
| `coverage-file` | Path to the coverage profile file | `coverage.out` |
| `config-file` | Path to the go-covercheck config file | _(CLI/config default)_ |
| `statement-threshold` | Global statement threshold percentage to enforce | _(CLI/config default)_ |
| `block-threshold` | Global block threshold percentage to enforce | _(CLI/config default)_ |
| `total-statement-threshold` | Total statement threshold percentage to enforce | _(CLI/config default)_ |
| `total-block-threshold` | Total block threshold percentage to enforce | _(CLI/config default)_ |
| `format` | Output format (table\|json\|yaml\|md\|html\|csv\|tsv) | _(CLI/config default)_ |
| `sort-by` | Column to sort results by | _(CLI/config default)_ |
| `sort-order` | Sort order (asc\|desc) | _(CLI/config default)_ |
| `skip` | Regex patterns of files/packages to skip (newline or comma separated) | _(CLI/config default)_ |
| `no-table` | Hide the results table (true\|false) | `false` |
| `no-summary` | Hide the summary (true\|false) | `false` |
| `no-color` | Disable color output (true\|false) | `false` |
| `term-width` | Terminal width override | _(CLI/config default)_ |
| `save-history` | Save the coverage result to history (true\|false) | `false` |
| `history-file` | Path to the history file | _(CLI/config default)_ |
| `label` | Label for the history entry | _(CLI/config default)_ |
| `compare-history` | Compare against a history entry (ref or label) | _(CLI/config default)_ |
| `show-history` | Show history instead of checking coverage (true\|false) | `false` |
| `limit-history` | Limit the number of history entries shown | _(CLI/config default)_ |
| `line-threshold` | Global line threshold percentage to enforce | _(CLI/config default)_ |
| `total-line-threshold` | Total line threshold percentage to enforce | _(CLI/config default)_ |
| `function-threshold` | Global function threshold percentage to enforce | _(CLI/config default)_ |
| `total-function-threshold` | Total function threshold percentage to enforce | _(CLI/config default)_ |
| `table-style` | Table style for the table output | _(CLI/config default)_ |
| `diff-from` | Only enforce coverage on code changed since this git ref | _(CLI/config default)_ |
| `module-name` | Go module name override | _(CLI/config default)_ |
| `no-uncovered-lines` | Hide uncovered lines (true\|false) | `false` |
| `no-uncovered-functions` | Hide uncovered functions (true\|false) | `false` |
| `comment` | Post results as a comment on the pull request (true\|false). Needs pull-requests write permission | `false` |
| `comment-token` | Token used to post the comment | `${{ github.token }}` |
| `version` | go-covercheck release to use (without or with leading v). Linux runs the ghcr.io/mach6/go-covercheck image, other runners build it with go install | `1.0.0` |
| `working-directory` | Directory (relative to the workspace) to run go-covercheck in | `.` |

## License

See [LICENSE](LICENSE).
