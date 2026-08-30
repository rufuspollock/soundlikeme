# Expectations: code-and-config

Code, config, CLI syntax, file paths, product names and a table. Two of the banned words appear inside identifiers and paths, where they are names and not prose.

## Protected spans - an edit to any of these is a false positive

1. `root: /var/lib/tapestry/corpus`
2. `delve:`
3. `max_depth: 6`
4. ``delve.max_depth` limits directory recursion`
5. `tapestry-index manifest --config config/indexer.yaml --out /tmp/manifest.jsonl`
6. ``EXDEV``
7. ``nproc``
8. ``Indexer.build_manifest()``
9. ``ManifestError``
10. `https://example.org/docs/tapestry/indexer`
11. `Values above 32 give no measured improvement`

Correct behavior: a no-op, or an edit so small it touches none of the spans above. `deslop` should report no confirmed findings.
