FROM gcr.io/cloud-builders/gsutil

COPY restore_cache /bin
COPY checksum /bin

ENTRYPOINT ["restore_cache"]