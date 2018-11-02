# unzip

This is a tool build to simply invoke the
[`unzip`](https://linux.die.net/man/1/unzip) command.

Arguments passed to this builder will be passed to `unzip` directly.

## Examples

The following examples demonstrate build requests that use this builder.

### Fetch the contents of a remote URL

This `cloudbuild.yaml` extracts a zip archive, overwriting any existing data.

```
steps:
- name: gcr.io/$PROJECT_ID/unzip
  args: ['-o', '-q', 'cache.zip']
```
