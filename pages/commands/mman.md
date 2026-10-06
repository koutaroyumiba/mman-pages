# mman

## NAME

`mman` - browse custom Markdown manual pages from the command line

## SYNOPSIS

```text
mman [OPTIONS] [TOPIC]
mman --list
mman --search TERM
```

## DESCRIPTION

`mman` discovers Markdown pages in one or more explicitly configured roots. A
topic is the page's path relative to its root without the final `.md`
extension. For example, `<root>/commands/mman.md` has the topic
`commands/mman`.

In a fully interactive terminal, running `mman` without a topic opens the topic
picker. Opening a topic starts the interactive viewer. When either standard
input or standard output is not an interactive terminal, a plain topic writes
its original Markdown to standard output instead.

Configure documentation roots with `MMANPATH` or use `-M` for one invocation.
Roots are searched from left to right, and the first copy of a topic is selected
by default.

## OPTIONS

- `-M PATHS` - replace `MMANPATH` for this invocation. It does not append to the
  environment value.
- `--raw` - write the selected page's original bytes to standard output. This
  preserves line endings and the presence or absence of a final newline.
- `--where` - write the selected page's source path to standard output.
- `-s`, `--select` - interactively select among every discovered source for a
  topic. A topic with only one source opens directly.
- `-l`, `--list` - list each unique topic once in stable alphabetical order.
- `-k TERM`, `--search TERM` - search topic names and page content.
- `-h`, `--help` - print command help.
- `-V`, `--version` - print the installed version.

`--raw`, `--where`, and `--select` require a topic. `--list` and `--search`
operate on the collection and do not accept a topic. Source selection requires
an interactive terminal.

## ENVIRONMENT

### `MMANPATH`

A platform-separated list of documentation roots. On macOS and Linux, separate
roots with `:`:

```sh
export MMANPATH="$HOME/manuals:$HOME/project/docs"
```

Earlier roots have higher precedence. Empty entries are ignored and do not mean
the current directory. Duplicate roots are removed without changing
precedence. A leading `~` is expanded only when it is a distinct first path
component, so `~/manuals` expands but `~someone/manuals` does not.

Missing, unreadable, and non-directory roots produce warnings while usable
roots continue to work. `mman` has no implicit documentation root and fails if
neither `MMANPATH` nor `-M` supplies a usable directory.

## TOPIC PICKER

Run `mman` without a topic in a fully interactive terminal to open the topic
picker. Filtering is case-insensitive and uses literal substring matching.

```text
Typing            Add characters to the filter
Backspace         Remove the last filter character
j / Down          Move to the next result
k / Up            Move to the previous result
Enter             Open the selected topic
Esc / q / Ctrl-C  Quit
```

Running `mman` without a topic in a non-interactive session is an error.

## SOURCE PICKER

Use `--select` to choose a source when the same topic exists in multiple roots:

```sh
mman --select commands/mman
```

The picker displays the full path of every source in precedence order. Use
`j`/`Down` and `k`/`Up` to move, `Enter` to open a source, and `Esc`, `q`, or
`Ctrl-C` to cancel. When only one source exists, it opens without displaying the
picker.

## INTERACTIVE VIEWER

Opening a topic in a fully interactive terminal starts the Markdown viewer. The
viewer reflows content when the terminal is resized and supports these keys:

```text
j / Down           Scroll down
k / Up             Scroll up
Ctrl-d / PageDown  Move down by one page
Ctrl-u / PageUp    Move up by one page
h / Left           Pan horizontally left
l / Right          Pan horizontally right
g / Home           Go to the beginning
G / End            Go to the end
/                  Enter in-page search
n                  Go to the next match
N                  Go to the previous match
Esc                Clear active search; quit if no search is active
?                  Toggle keybinding help
q                  Quit
Ctrl-C             Quit
```

In-page search is case-insensitive. Type a query after `/`, press `Enter` to
submit it, and use `n` or `N` to move through highlighted matches. Pressing
`Esc` while entering a query cancels the new query; pressing it while an active
search is displayed clears that search.

## COLLECTION SEARCH

`--search` performs case-insensitive literal matching over topic names and raw
Markdown content:

```sh
mman --search "documentation root"
```

Results are ordered by topic. Each result includes up to three matching source
lines with one-based line numbers. Only the highest-precedence copy of each
topic is searched. Collection search requires pages to contain valid UTF-8.

## PAGE DISCOVERY

Only visible files with the lowercase `.md` extension are discovered. Hidden
files and hidden directories are ignored. Symlinked Markdown files are
included, but symlinked directories are not traversed.

Topic names are derived from relative paths:

```text
<root>/git.md                       -> git
<root>/languages/rust.md            -> languages/rust
<root>/concepts/ownership/index.md  -> concepts/ownership
```

A root-level `index.md` does not produce a topic. A topic may be entered with or
without its final `.md` extension.

Topic lookup is exact and case-sensitive. Absolute topics and topics containing
hidden components, `.`, `..`, repeated separators, or control characters are
rejected.

### Precedence

When multiple files resolve to the same topic, precedence is:

1. Earlier configured roots before later roots.
2. A direct page such as `ownership.md` before `ownership/index.md` within the
   same root.
3. Stable path order as a deterministic tie-breaker.

Root precedence is stronger than page form. An index page in an earlier root
therefore wins over a direct page in a later root. All copies remain available
through `--select`.

## OUTPUT

Page content, topic lists, collection-search results, and `--where` results are
written to standard output. Warnings and errors are written to standard error.
This allows raw page output to be redirected or piped without mixing in
diagnostics.

`--raw` does not require UTF-8 and preserves the file bytes exactly. Interactive
viewing and collection search require valid UTF-8.

## EXIT STATUS

- `0` - the requested operation completed successfully or an interactive picker
  was cancelled normally.
- Non-zero - usage was invalid, no usable root was configured, a topic was not
  found, a page could not be read, text was not valid UTF-8 where required, or
  an interactive operation was unavailable.

## EXAMPLES

Configure this repository and open this page:

```sh
export MMANPATH="$HOME/workspace/mman-pages/pages"
mman commands/mman
```

List and search the collection:

```sh
mman --list
mman --search "MMANPATH"
```

Print Markdown for use in a pipeline:

```sh
mman --raw commands/mman
mman commands/mman | grep MMANPATH
```

Print the selected source path:

```sh
mman --where commands/mman
```

Temporarily search multiple roots without changing `MMANPATH`:

```sh
mman -M "$HOME/workspace/mman-pages/pages:$HOME/project/docs" \
  --where commands/mman
```

## ERRORS AND PITFALLS

- `-M` replaces `MMANPATH`; it does not append to it.
- Topic lookup is case-sensitive even though picker filtering, collection
  search, and in-page search are case-insensitive.
- Root order takes precedence over direct-page versus index-page form.
- `--select` requires both standard input and standard output to be interactive.
- A plain topic emits raw Markdown when either standard input or standard output
  is not interactive.
- Empty `MMANPATH` entries are ignored rather than interpreted as the current
  directory.
- Markdown is treated as data. `mman` does not execute code, interpret embedded
  HTML, fetch remote resources, or invoke external renderers.

## NOTES

This page documents `mman` 0.1.0 on its supported macOS and Linux platforms.
Behavior and options may change in later versions.
