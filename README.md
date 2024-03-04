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

Get Total Space of all buckets in a project, human readable and as summary
```
gsutil  du -shc

# Or, to specify a different project:
gsutil -o GSUtil:default_project_id=<Project ID> du -shc
```


## CloudSQL
Stop an instance
```
gcloud sql instances patch INSTANCE_NAME \
--activation-policy=NEVER
```

## Biqquery
### bq cli
Create a new table
bq mk --table [PROJECT]:[DATASET].[TABLE] [path to json schema file] 
example:
```
bq mk --table  vmsilva-sandbox-company:sells_dataset.my_new_table  ./table_shema.json
```

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

BQ tables metadata and Information Schema  
https://github.com/GoogleCloudPlatform/training-data-analyst/blob/master/courses/data-engineering/demos/information_schema.md



### BQ - Logging
Get BQ iteractions by user
```
resource.type="bigquery_resource"
protoPayload.authenticationInfo.principalEmail="user@domain.com"
```


## Logging
https://cloud.google.com/logging/docs/overview
### Logging Types:
- Platform Logs
- Component Logs
- Security Logs
  - Cloud Audit
  - Access Transparency
- User-written logs
- Multi-cloud / Hybrid.Cloud logs

### Audit Logging
#### Project Events
Get the event of project creation.  
Could be queried at Project, Folder or Organization Level.
```
protoPayload.methodName="CreateProject"
protoPayload.resourceName="projects/[PROJECT-ID]"  (Optional)
```
Get the event of project creation by user.  
Could be queried at Project, Folder or Organization Level.
```
protoPayload.methodName="CreateProject"
protoPayload.authenticationInfo.principalEmail="[USER EMAIL]"
```

## Google Curl
### use gcurl alias to authenticate curl requests:
```
alias gcurl='curl -H "Authorization: Bearer $(gcloud auth print-access-token)" -H "Content-Type: application/json"'
```
### Force GCP quota project
To specify the quota project (the project that will receive the request, and also be charged) the gcloud command **gcloud auth application-default set-quota-project [PROJECT-ID]** shuold work, however, sometimes even after running this command, the curl command does not uses the desired project. 
To force the quota project in request, use the following header: **X-Goog-User-Project**  
Example:
```
curl -H "X-Goog-User-Project: [project-id]" ...
```


## Costs
### BQ queries on Billing export dataset

Get all costs on a specific data
```
SELECT * FROM `[Project].[Dataset].gcp_billing_export_resource_v1_[Billing Account ID]` 
WHERE TIMESTAMP_TRUNC(_PARTITIONTIME, DAY) = TIMESTAMP("2024-01-23") LIMIT 1000
```



