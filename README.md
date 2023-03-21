# GCP-Cheat-Sheet-
Google CLoud Platform Cheat Sheet 

## GCS - Google Cloud Storage
List Bucket files
```
gsutil ls gs://example-bucket
```

Get Bucket files size (file by file)
```
gsutil du gs://bi4all-demo-bq-global-raw-data
```

Get Bucket files size, human readable and as summary
```
gsutil du -s -h gs://bi4all-demo-bq-global-raw-data
```
