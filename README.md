# Google Cloud Build Android Utilities

This repository contains utilities to build Android apps using [Google Cloud Build](https://cloud.google.com/cloud-build/).

# Usage

### 1. Deploy the builders

Depending on your needs, you will need to deploy some or all of the supplied builders to Google Cloud Registry.  You can deploy all builders using teh following command.

```bash
gcloud builds submit --config=cloudbuild.yaml
```

Check the Android [readme file](android/README.md) for instructions to build the Android SDK image.

### 2. Deploy the Github Status Cloud Function (Optional)

If you want to get build notifications in Github then follow the instructions in the [github-status](github-status/README.md) project to deploy the Cloud Function.

### 3. Create a cloudbuild.yaml file

Create a `cloudbuild.yaml` file based on your needs and save it in the root of your project.

Check out the [complete sample](samples/build-test-deploy.yaml) for some ideas of how to accomplish this.

### 4. Encrypt your secret files (Optional)

If you have files that should be kept secret, like keystores or service account files, you can encrypt them in your source repository and decrypt them at build time.

```bash
# Create a keyring to store your keys
gcloud kms keyrings create my-app --location=global

# Create a key to store your secrets
gcloud kms keys create android-builder --location=global --keyring=my-app --purpose=encryption

# Add the service account for your gcloud project to the encrypt/decrypt role
gcloud kms keys add-iam-policy-binding android-builder --location=global --keyring=my-app --member='serviceAccount:1234567890@cloudbuild.gserviceaccount.com' --role=roles/cloudkms.cryptoKeyEncrypterDecrypter

# Encrypt all the things
gcloud kms encrypt --plaintext-file=signing/keystore.properties --ciphertext-file=signing/keystore.properties.enc --location=global --keyring=my-app --key=android-builder
gcloud kms encrypt --plaintext-file=signing/google-service-account.json --ciphertext-file=signing/google-service-account.json.enc --location=global --keyring=my-app --key=android-builder
```

Be sure to add your plain text files to your `.gitignore` file so they aren't uploaded to your repository.

### 5. Create the required cloud buckets (Optional)

If you want to store artifacts, configs, or your build cache, you'll need to create the cloud buckets to do so.

To create buckets to support the [sample config](samples/build-test-deploy.yaml), run the following commands.

```bash
gsutil mb gs://my-build-artifacts
gsutil mb gs://my-build-config
gsutil mb gs://my-build-cache
```

### 6. Add your Trigger

In order for Google Cloud Build to automatically build your app when commits are pushed to Github, you'll have to create a trigger.  This can only be done via the [Cloud Build Web Interface](https://console.cloud.google.com/cloud-build/triggers).

Choose a name, specify a branch or tag, and fill in the substitution variables.

Substitution variables depend on your `cloudbuild.yaml` configuration, but the example configuration uses the following:

| `_ARTIFACT_BUCKET` | my-build-artifacts |
| `_CACHE_BUCKET` | my-build-cache |
| `_CONFIG_BUCKET` | my-build-config |

### 7. Push some code

That's it!  All that's left is to push your code to Github and Cloud Build will start building your code, 

### 8. Running Locally

That's not really it!  You can submit builds locally, without having to push to Github, using the gcloud command line tool.

```bash
gcloud builds submit --config=cloudbuild.yaml --substitutions=_CONFIG_BUCKET=my-app-config,_ARTIFACT_BUCKET=my-app-artifacts,_CACHE_BUCKET=my-app-cache
```

You can also build and debug locally using the [cloud-build-local](https://cloud.google.com/cloud-build/docs/build-debug-locally) command.

```bash
cloud-build-local --config=cloudbuild.yaml --substitutions=_CONFIG_BUCKET=my-app-config,_ARTIFACT_BUCKET=my-app-artifacts,_CACHE_BUCKET=my-app-cache .
```

## License

```
Copyright 2018 Pixite Inc.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
