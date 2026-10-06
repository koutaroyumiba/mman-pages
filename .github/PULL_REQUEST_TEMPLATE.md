<!-- Give the PR a concise title that summarizes the overall contribution. -->

## Summary

<!-- Describe the reader-facing change and why it is needed. -->

## Topics Changed

<!-- List every affected topic path, or write "None" for repository-only work. -->

- `path/to/topic`

## Change Type

- [ ] New page or substantial addition
- [ ] Correction
- [ ] Topic rename, move, or removal
- [ ] Repository documentation
- [ ] CI or maintenance
- [ ] Release

## Commit Structure

- [ ] Each commit changes one primary page or performs one focused repository action.
- [ ] Each page's changelog entry is included in the same commit as that page change.
- [ ] Multi-page commits have been split into page-level commits.
- [ ] The branch is based on the latest `main` and is ready for a rebase merge.

<!-- Explain any unavoidable exception to the commit policy. -->

## Compatibility

- [ ] Additive; existing topic paths and meanings remain valid
- [ ] Corrective; existing topic paths remain valid
- [ ] Breaking; migration instructions and a major-version release are included
- [ ] Not applicable; no published page behavior changes

<!-- Explain any compatibility or versioning implications. -->

## Verification

<!-- Check the applicable items. Remove or explain items that do not apply. -->

- [ ] Topic paths are stable, lowercase, and appropriately namespaced.
- [ ] New pages contain `NAME`, `SYNOPSIS`, and `DESCRIPTION`.
- [ ] Examples have been run or otherwise verified where practical.
- [ ] Claims, edge cases, and complexity bounds have been checked.
- [ ] Version-specific behavior identifies the relevant version.
- [ ] Related topics appear under `SEE ALSO` where relevant.
- [ ] Pages are readable as plain Markdown and render correctly in `mman`.
- [ ] Relative links resolve within `pages/`.
- [ ] Relevant `mman -M pages` lookup, listing, and search checks pass.
- [ ] `CHANGELOG.md` is updated under `Unreleased` for reader-visible changes.

## Release Checklist

<!-- Complete this section only for a release pull request. -->

- [ ] Completed entries are in a dated version section.
- [ ] A new empty `Unreleased` section remains.
- [ ] The version increment matches the changes.
- [ ] The full diff from the previous release tag has been reviewed.
- [ ] Required sections, duplicate topics, links, examples, and whitespace have been checked.
- [ ] The intended `mman` version successfully discovers, opens, lists, and searches the collection.

## Additional Notes

<!-- Include sources, review questions, or follow-up work. -->
