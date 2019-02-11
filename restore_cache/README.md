# restore_cache

Restores the cache from the archive file defined by the provided `key`, either from the local `src` directory, or the remote GCS `bucket`

## Examples

The following examples demonstrate build requests that use this builder.

### Restore a cache from 

This `cloudbuild.yaml` restores the files from the compressed cache file identified by `key` on the cache bucket provided, if it exists.

```yaml
- name: 'gcr.io/$PROJECT_ID/restore_cache'
  args:
  - '--bucket=gs://$CACHE_BUCKET/'
  - '--key=resources-$( checksum cloudbuild.yaml )'
  volumes:
  - name: 'cache'
    path: '/cache'
```
