# zip

This is a tool build to simply invoke the
[`zip`](https://linux.die.net/man/1/zip) command.

Arguments passed to this builder will be passed to `zip` directly.

## Examples

The following examples demonstrate build requests that use this builder.

### Fetch the contents of a remote URL

This `cloudbuild.yaml` zips the gradle build cache directory.

```
steps:
- name: gcr.io/$PROJECT_ID/zip
  args: ['-q', '-r', 'cache.zip', '.gradle']
```
