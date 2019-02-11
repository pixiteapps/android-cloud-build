# save_cache

Saves the provided cache files to a compresssed archive, optionally uploading it to the provided cloud storage bucket.

## Examples

The following examples demonstrate build requests that use this builder.

### Saving a cache with checksum to GCS bucket

This `cloudbuild.yaml` saves the files and folders in the `path` arguments to a cache file in the GCS bucket `gs://$CACHE_BUCKET/`.  In this example the key will be updated, resulting in a new cache, every time the `cloudbuild.yaml` build file is changed.

```yaml
- name: 'gcr.io/$PROJECT_ID/save_cache'
  args:
  - '--bucket=gs://$CACHE_BUCKET/'
  - '--key=resources-$( checksum cloudbuild.yaml )'
  - '--path=/cache/folder1'
  - '--path=/cache/folder2/subfolder3'
  volumes:
  - name: 'cache'
    path: '/cache'
```
