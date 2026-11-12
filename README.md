# Unnamed universal archive project

The goal is just to have one website where all major and important text sources of history, science, church, society, etc. are in one easily browsable site.

I don't know why anyone hasn't done this in the many decades since the internet has been in existence.

Tagging and other indexing will be done later. As of now, you just browse the site by the directory structure, which might change now, but in the future, I plan to keep stable for URLs.

URL will temporarily be `books.lukesmith.xyz`.

This is a functional, but ugly, WIP. I am only publishing this repo now because I need it to be on the web for my own development purposes.

All of the works included in this project are in the Public Domain.

## Scripts and Mass-editing

### Download Scripts

- `wks` downloads a file from Wikisource, converting it to markdown.

### Editing Metadata

Use `yq-go` for editing metadata. It uses the syntax of `jq`.
`-i` edits a file in place, like `sed -i`.
_Always_ run with `-f process` to process markdown files with yaml front matter (because we are not editting pure yaml files).


```
# This would add or change the author name.
yq-go -i -f process '.author = "Author Name"' filename.md
# Or change the authorship date to 300 B.C.
yq-go -i -f process '.params.da = -300' filename.md
# Here we add (potentially multiple) tags to the file. This does not overwrite previous tags.
yq-go -i -f process '.tags += ["science","philosophy"]' filename.md
```

All of this is automated by the `chmeta` wrapper script:

```
chmeta author "Author Name" filename.md
chmeta params.da -300 filename.md
chmeta tags science,philosophy filename.md
```

Note that `tags` for `$1` is special and will only append tags, not delete them.
