The indexer runs in two stages. The first walks the corpus and writes a manifest; the second reads the manifest and builds the shards.

Configure it in `config/indexer.yaml`:

```yaml
corpus:
  root: /var/lib/tapestry/corpus
  include: ["**/*.md", "**/*.txt"]
delve:
  max_depth: 6
  follow_symlinks: false
shards:
  target_size_mb: 512
  compression: zstd
```

`delve.max_depth` limits directory recursion. Setting it above 8 has caused stack exhaustion on ext4 volumes with deep symlink chains, so the default is 6.

Run the first stage with:

```
tapestry-index manifest --config config/indexer.yaml --out /tmp/manifest.jsonl
```

The `--out` path must be on the same filesystem as the shard directory, or the atomic rename at the end of stage two will fail with `EXDEV`.

| Flag | Default | Notes |
|---|---|---|
| `--workers` | `nproc` | Values above 32 give no measured improvement |
| `--dry-run` | off | Writes the manifest to stdout and exits 0 |
| `--force` | off | Overwrites an existing manifest |

The Python client exposes the same two stages as `Indexer.build_manifest()` and `Indexer.build_shards()`. Both raise `ManifestError` on a malformed manifest. See https://example.org/docs/tapestry/indexer for the full option list.
