## Summary

<!-- Describe the reader-facing change. -->

## Change Type

- [ ] New page
- [ ] Content addition
- [ ] Correction
- [ ] Topic rename, move, or removal
- [ ] Repository/documentation maintenance

## Verification

Before publishing a page:

- [ ] The topic path is stable, lowercase, and appropriately namespaced.
- [ ] `NAME`, `SYNOPSIS`, `DESCRIPTION` are present.
- [ ] The page has one primary subject.
- [ ] Examples have been run or otherwise verified where practical.
- [ ] Claims, edge cases, and complexity bounds have been checked.
- [ ] Version-specific behaviour identifies the relevant version.
- [ ] Related topics appear under `SEE ALSO`.
- [ ] The page is readable as plain Markdown and renders correctly in `mman`.
- [ ] Relative links resolve within `pages/`.
- [ ] The change is recorded under `Unreleased` in `CHANGELOG.md`.
- [ ] Existing topic paths remain valid, or the release is marked as breaking.

Before publishing a collection release:

- [ ] Move `Unreleased` entries into a dated version section.
- [ ] Confirm that the version increment matches the changes.
- [ ] Review the diff from the previous release tag.
- [ ] Verify lookup, listing, and search with the intended `mman` version.
- [ ] Check required sections, duplicate topics, relative links, and trailing whitespace.
- [ ] Create an annotated release tag.
- [ ] Do not modify the tag after publication.

## Additional Notes

<!-- include sources, compatibility concerns, or follow-up work -->
