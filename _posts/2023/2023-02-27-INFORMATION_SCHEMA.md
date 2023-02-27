---
layout: single
title: "INFORMATION_SCHEMA"
categories: [GCP]
tag: [BigQuery, INFORMATION_SCHEMA]
toc: true
sidebar:
    nav: "docs"
search: true
---

## BigQuery INFORMATION_SCHEMA 소개

`BigQuery INFORMATION_SCHEMA 뷰는 읽기 전용이며 BigQuery 객체에 대한 메타데이터 정보를 제공하는 시스템 정의 뷰입니다.` 



## REFERENCE

|INFORMATION_SCHEMA 뷰|리소스 유형|
|--- |--- |
|[INFORMATION_SCHEMA.SCHEMATA](https://cloud.google.com/bigquery/docs/information-schema-intro?hl=ko#schemata_view)|데이터 세트|
|[INFORMATION_SCHEMA.SCHEMATA_OPTIONS](https://cloud.google.com/bigquery/docs/information-schema-intro?hl=ko#schemata_options_view)|데이터 세트 옵션|
|[INFORMATION_SCHEMA.JOBS_BY_*](https://cloud.google.com/bigquery/docs/information-schema-jobs?hl=ko)|작업|
|[INFORMATION_SCHEMA.JOBS_TIMELINE_BY_*](https://cloud.google.com/bigquery/docs/information-schema-jobs-timeline?hl=ko)|작업 타임라인|
|[INFORMATION_SCHEMA.OBJECT_PRIVILEGES](https://cloud.google.com/bigquery/docs/information-schema-object-privileges?hl=ko)|액세스 제어|
|[INFORMATION_SCHEMA.RESERVATION*](https://cloud.google.com/bigquery/docs/information-schema-reservations?hl=ko)|예약|
|[INFORMATION_SCHEMA.CAPACITY_COMMITMENT*](https://cloud.google.com/bigquery/docs/information-schema-capacity-commitments?hl=ko)|예약 용량 약정|
|[INFORMATION_SCHEMA.ASSIGNMENT*](https://cloud.google.com/bigquery/docs/information-schema-assignments?hl=ko)|예약 할당|
|[INFORMATION_SCHEMA.ROUTINES](https://cloud.google.com/bigquery/docs/information-schema-routines?hl=ko)|루틴|
|[INFORMATION_SCHEMA.ROUTINE_OPTIONS](https://cloud.google.com/bigquery/docs/information-schema-routine-options?hl=ko)|루틴 옵션|
|[INFORMATION_SCHEMA.PARAMETERS](https://cloud.google.com/bigquery/docs/information-schema-parameters?hl=ko)|루틴 매개변수|
|[INFORMATION_SCHEMA.STREAMING_TIMELINE_BY_*](https://cloud.google.com/bigquery/docs/information-schema-streaming?hl=ko)|스트리밍 데이터|
|[INFORMATION_SCHEMA.TABLE*](https://cloud.google.com/bigquery/docs/information-schema-tables?hl=ko)|테이블|
|[INFORMATION_SCHEMA.COLUMN*](https://cloud.google.com/bigquery/docs/information-schema-columns?hl=ko)|테이블 열|
|[INFORMATION_SCHEMA.PARTITIONS](https://cloud.google.com/bigquery/docs/information-schema-partitions?hl=ko)|테이블 파티션|
|[INFORMATION_SCHEMA.TABLE_SNAPSHOTS](https://cloud.google.com/bigquery/docs/information-schema-snapshots?hl=ko)|테이블 스냅샷|
|[INFORMATION_SCHEMA.VIEWS](https://cloud.google.com/bigquery/docs/information-schema-views?hl=ko)|뷰|
|[INFORMATION_SCHEMA.SESSIONS_BY_*](https://cloud.google.com/bigquery/docs/information-schema-sessions?hl=ko)|세션|
|[TABLE META DATA](https://stackoverflow.com/questions/44288261/get-the-last-modified-date-for-all-bigquery-tables-in-a-bigquery-project)|메타데이터|



## META DATA EXAMPLE 
```sql
#standardSQL
SELECT *, TIMESTAMP_MILLIS(last_modified_time) last_modified_timestamp
FROM `dataset.__TABLES__` where table_id like 'table_name%'
```


| project_id      | dataset_id      | table_id      | creation_time | last_modified_time | row_count | size_bytes | type | last_modified_timestamp        |
| --------------- | --------------- | ------------- | ------------- | ------------------ | --------- | ---------- | ---- | ------------------------------ |
| your_project_id | your_dataset_id | your_table_id | 1675647106802 | 1675647127420      | 4330      | 336088     | 1    | 2023-02-06 01:32:07.420000 UTC |
| your_project_id | your_dataset_id | your_table_id | 1675647051287 | 1675647102301      | 5812      | 451295     | 1    | 2023-02-06 01:31:42.301000 UTC |
| your_project_id | your_dataset_id | your_table_id | 1675647025282 | 1675647048880      | 4953      | 384616     | 1    | 2023-02-06 01:30:48.880000 UTC |
| your_project_id | your_dataset_id | your_table_id | 1675647751604 | 1675647769644      | 4518      | 350798     | 1    | 2023-02-06 01:42:49.644000 UTC |
| your_project_id | your_dataset_id | your_table_id | 1675647731291 | 1675647749110      | 4525      | 351292     | 1    | 2023-02-06 01:42:29.110000 UTC |
| your_project_id | your_dataset_id | your_table_id | 1675647709909 | 1675647728649      | 4942      | 383588     | 1    | 2023-02-06 01:42:08.649000 UTC |



## TABLE CONVERTER
- [html to markdown](https://jmalarcon.github.io/markdowntables/)
- [excel to markdown](https://tabletomarkdown.com/convert-spreadsheet-to-markdown/)


## 출처
- https://www.bespinglobal.com/bigquery_information_schema/
- https://medium.com/google-cloud/bigquery-dataset-metadata-queries-8866fa947378