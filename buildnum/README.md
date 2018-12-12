# buildnum

Script to automatically read and increment a build nubmer from a file, saving it to another file that can be sourced to add the `BUILD_NUM` env variable.

Requires 2 arguments, the buildnum file path to be persisted between builds, and the path to store the build environment.

## Examples

The following examples demonstrate build requests that use this builder.

### Load build number

This `cloudbuild.yaml` loads a build number from a shared file.

```
- name: 'gcr.io/$PROJECT_ID/buildnum'
  args:
  - '/config/buildnum'
  - 'build_env'
  volumes:
  - name: 'config'
    path: '/config'
```
