# GCP-Cheat-Sheet-
Google CLoud Platform Cheat Sheet 

## GCS - Google Cloud Storage
List Bucket files
```
gsutil ls gs://example-bucket
```

Get Bucket files size (file by file)
```
gsutil du gs://company-demo-bq-global-raw-data
```

Get Bucket files size, human readable and as summary
```
gsutil du -s -h gs://company-demo-bq-global-raw-data
```


## Biqquery
### bq cli
Load from GCS into BQ, specifying source format
```
bq load --source_format=[file format] [dataset].[table] [source GCS Path]
# examples:
bq load --source_format=AVRO ecommerce.order_items   gs://company-demo-bq-global-raw-data/raw_avro/order_items/*
bq load --source_format=PARQUET ecommerce.order_items   gs://company-demo-bq-global-raw-data/raw_avro/order_items/*
```


