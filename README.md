# GCP Cheat Sheet
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

## CloudSQL
Stop an instance
```
gcloud sql instances patch INSTANCE_NAME \
--activation-policy=NEVER
```

## Biqquery
### bq cli
Load from GCS into BQ, specifying source format
```
bq load --source_format=[file format] [dataset].[table] [source GCS Path]
# examples:
bq load --source_format=AVRO ecommerce.order_items   gs://company-demo-bq-global-raw-data/raw_avro/order_items/*
bq load --source_format=PARQUET ecommerce.order_items   gs://company-demo-bq-global-raw-data/raw_parquet/order_items/*
```
to specify project id, should be used --project_id  
(bq load differs from standar gcloud, where is simply used --project.)
```
bq load --project_id [Project ID] (...)
# example
bq load --project_id vmsilva-sandbox-company --source_format=PARQUET company_training_raw.Sales   gs://dataproc_output_files/COMPANY_Training/Sales/*.parquet
```

Export BQ schema to json
```
bq show --format=prettyjson [PROJECT]:[DATASET].[TABLE] | jq '.schema.fields'
Example:
bq show --format=prettyjson bigquery-public-data:samples.wikipedia | jq '.schema.fields'
```

