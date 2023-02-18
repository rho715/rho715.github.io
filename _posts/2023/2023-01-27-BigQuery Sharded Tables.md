---
layout: single
title: "BigQuery Sharded Tables"
categories: [GCP]
tag: [BigQuery, SQL, sharded, yyyymmdd, offset, regex, ddl, INFORMATION_SCHEMA]
toc: false
author_profile: false
sidebar:
    nav: "docs"
search: true
---

#### Using Array 

```python
from google.cloud import bigquery

location = "asia-northeast3"
my_account = "# rho715"

def runQuery_DF(query):
    final_query = "\n".join([my_account, query]) #add my account info to query input

    client = bigquery.Client(location=location)
    data = client.query(final_query).to_dataframe() #run query
    client.close()

    print('\nquery : ' , final_query)
    return data

query = """
with data_struct as (
  SELECT
    [struct('{project_id}.{dataset}.{table_name}' as table_address, '' as table_type )
    , struct('{project_id}.{dataset}.{table_name}_20220101' as table_address, 'sharded' as table_type )
    ] as sample_table
)
SELECT
    SPLIT(sample_table_elem.table_address, ".")[OFFSET(0)] AS project,
    SPLIT(sample_table_elem.table_address, ".")[OFFSET(1)] AS dataset,
    SPLIT(sample_table_elem.table_address, ".")[OFFSET(2)] AS table,
    sample_table_elem.table_address,
    REGEXP_REPLACE(table_address, r"(20[0-9]{6})", "{yyyymmdd}") param, -- change 20220101 -> {yyyymmdd} : for parameter_use
    regexp_extract((table_address), r"(20[0-9]{6})") yyyymmdd,
    sample_table_elem.table_type,
FROM data_struct, UNNEST(sample_table) as sample_table_elem
"""
runQuery_DF(query)

```

  



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>project</th>
      <th>dataset</th>
      <th>table</th>
      <th>table_address</th>
      <th>param</th>
      <th>yyyymmdd</th>
      <th>table_type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>{project_id}</td>
      <td>{dataset}</td>
      <td>{table_name}</td>
      <td>{project_id}.{dataset}.{table_name}</td>
      <td>{project_id}.{dataset}.{table_name}</td>
      <td>None</td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>{project_id}</td>
      <td>{dataset}</td>
      <td>{table_name}_20220101</td>
      <td>{project_id}.{dataset}.{table_name}_20220101</td>
      <td>{project_id}.{dataset}.{table_name}_{yyyymmdd}</td>
      <td>20220101</td>
      <td>sharded</td>
    </tr>
  </tbody>
</table>
</div>


#### Using `INFORMATION_SCHEMA.TABLES`

```python
from google.cloud import storage, bigquery

location = "asia-northeast3"
my_account = "# rho715"

def runQuery_DF(query):
    final_query = "\n".join([my_account, query]) #add my account info to query input

    client = bigquery.Client(location=location)
    data = client.query(final_query).to_dataframe() #run query
    client.close()

    print('\nquery : ' , final_query)
    return data

query = """
-- DECLARE d_yyyymmdd STRING DEFAULT '20230101';
DECLARE d_yyyymmdd STRING DEFAULT FORMAT_DATE("%Y%m%d", CURRENT_DATE("Asia/Seoul"));

with sharded_table_filter as (
  select REGEXP_REPLACE(table_name, r"(20[0-9]{6})", "{yyyymmdd}") table_name
        , REGEXP_REPLACE(max(table_name), r"(20[0-9]{6})", d_yyyymmdd ) max_table_name --max(table_name) max_table_name
        , regexp_extract(max(table_name), r"(20[0-9]{6})") yyyymmdd
  from {input_dataset}.INFORMATION_SCHEMA.TABLES
  group by table_name
) -- 샤딩테이블은 가장 최신 테이블을 불러온다음 날짜를 "{yyyymmdd}"로 바꿈
select 
      A.table_schema,
      stf.table_name,
      A.column_name,
      A.DATA_TYPE,
      C.description,
      B.table_type,
      is_partitioning_column,
      '' as sample_data,
      ordinal_position,
      is_nullable,
      clustering_ordinal_position,
      stf.max_table_name
from {input_dataset}.INFORMATION_SCHEMA.COLUMNS A
inner join sharded_table_filter stf on A.table_name = stf.max_table_name
left join {input_dataset}.INFORMATION_SCHEMA.TABLES B on A.table_name = B.table_name
left join {input_dataset}.INFORMATION_SCHEMA.COLUMN_FIELD_PATHS C on concat(A.table_name, A.column_name) = concat(C.table_name,C.column_name)
order by 1,2;
"""
runQuery_DF(query)

```
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>table_schema</th>
      <th>table_name</th>
      <th>column_name</th>
      <th>DATA_TYPE</th>
      <th>description</th>
      <th>table_type</th>
      <th>is_partitioning_column</th>
      <th>sample_data</th>
      <th>ordinal_position</th>
      <th>is_nullable</th>
      <th>clustering_ordinal_position</th>
      <th>max_table_name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>{input_dataset}</td>
      <td>log_{yyyymmdd}</td>
      <td>uno</td>
      <td>STRING</td>
      <td>user number</td>
      <td>BASE TABLE</td>
      <td>NO</td>
      <td></td>
      <td>1</td>
      <td>YES</td>
      <td>&lt;NA&gt;</td>
      <td>log_20230127</td>
    </tr>
    <tr>
      <th>1</th>
      <td>{input_dataset}</td>
      <td>log_{yyyymmdd}</td>
      <td>profile_id</td>
      <td>STRING</td>
      <td>user profile id</td>
      <td>BASE TABLE</td>
      <td>NO</td>
      <td></td>
      <td>2</td>
      <td>YES</td>
      <td>&lt;NA&gt;</td>
      <td>log_20230127</td>
    </tr>
    <tr>
      <th>2</th>
      <td>{input_dataset}</td>
      <td>log_{yyyymmdd}</td>
      <td>device_type</td>
      <td>STRING</td>
      <td>클라이언트 단말 구분</td>
      <td>BASE TABLE</td>
      <td>NO</td>
      <td></td>
      <td>3</td>
      <td>YES</td>
      <td>&lt;NA&gt;</td>
      <td>log_20230127</td>
    </tr>
    <tr>
      <th>3</th>
      <td>{input_dataset}</td>
      <td>log_{yyyymmdd}</td>
      <td>program_id</td>
      <td>STRING</td>
      <td>program id</td>
      <td>BASE TABLE</td>
      <td>NO</td>
      <td></td>
      <td>4</td>
      <td>YES</td>
      <td>&lt;NA&gt;</td>
      <td>log_20230127</td>
    </tr>
    <tr>
      <th>4</th>
      <td>{input_dataset}</td>
      <td>log_{yyyymmdd}</td>
      <td>content_id</td>
      <td>STRING</td>
      <td>content id</td>
      <td>BASE TABLE</td>
      <td>NO</td>
      <td></td>
      <td>5</td>
      <td>YES</td>
      <td>&lt;NA&gt;</td>
      <td>log_20230127</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>71</th>
      <td>{input_dataset}</td>
      <td>event_{yyyymmdd}</td>
      <td>guid</td>
      <td>STRING</td>
      <td>클라이언트 단말 고유ID</td>
      <td>BASE TABLE</td>
      <td>NO</td>
      <td></td>
      <td>3</td>
      <td>YES</td>
      <td>&lt;NA&gt;</td>
      <td>event_20230127</td>
    </tr>
    <tr>
      <th>72</th>
      <td>{input_dataset}</td>
      <td>event_{yyyymmdd}</td>
      <td>params</td>
      <td>STRING</td>
      <td>http params</td>
      <td>BASE TABLE</td>
      <td>NO</td>
      <td></td>
      <td>4</td>
      <td>YES</td>
      <td>&lt;NA&gt;</td>
      <td>event_20230127</td>
    </tr>
    <tr>
      <th>73</th>
      <td>{input_dataset}</td>
      <td>event_{yyyymmdd}</td>
      <td>bodies</td>
      <td>STRING</td>
      <td>JSON 형식의 uievent 로그 데이터</td>
      <td>BASE TABLE</td>
      <td>NO</td>
      <td></td>
      <td>5</td>
      <td>YES</td>
      <td>&lt;NA&gt;</td>
      <td>event_20230127</td>
    </tr>
    <tr>
      <th>74</th>
      <td>{input_dataset}</td>
      <td>event_{yyyymmdd}</td>
      <td>ap_date</td>
      <td>STRING</td>
      <td>로그 서버의 로그 수신 시간(KST)</td>
      <td>BASE TABLE</td>
      <td>NO</td>
      <td></td>
      <td>6</td>
      <td>YES</td>
      <td>&lt;NA&gt;</td>
      <td>event_20230127</td>
    </tr>
    <tr>
      <th>75</th>
      <td>{input_dataset}</td>
      <td>event_{yyyymmdd}</td>
      <td>ap_timestamp</td>
      <td>TIMESTAMP</td>
      <td>ap_date의 timestamp 값(KST)</td>
      <td>BASE TABLE</td>
      <td>YES</td>
      <td></td>
      <td>7</td>
      <td>YES</td>
      <td>&lt;NA&gt;</td>
      <td>event_20230127</td>
    </tr>
  </tbody>
</table>
<p>76 rows × 12 columns</p>
</div>


#### Using ddl 

```python
from google.cloud import storage, bigquery

location = "asia-northeast3"
my_account = "# rho715"

def runQuery_DF(query):
    final_query = "\n".join([my_account, query]) #add my account info to query input

    client = bigquery.Client(location=location)
    data = client.query(final_query).to_dataframe() #run query
    client.close()

    print('\nquery : ' , final_query)
    return data

query = """
select  table_catalog
    , table_schema
    , table_name
    , current_date as yyyy_mm_dd
    , ddl
    , replace(ddl, 'CREATE TABLE', 'CREATE TABLE IF NOT EXISTS') ddl_create_if_not_exists 
  from {input_project_id}.{input_dataset}.INFORMATION_SCHEMA.TABLES t2
  order by 1 ,3
  limit 5
"""
runQuery_DF(query)

```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>table_catalog</th>
      <th>table_schema</th>
      <th>table_name</th>
      <th>yyyy_mm_dd</th>
      <th>ddl</th>
      <th>ddl_create_if_not_exists</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>{input_project_id}</td>
      <td>{input_dataset}</td>
      <td>log_20230125</td>
      <td>2023-01-27</td>
      <td>CREATE TABLE `{input_project_id}.{input_dataset}.log_2023...</td>
      <td>CREATE TABLE IF NOT EXISTS `{input_project_id}.{input_data...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>{input_project_id}</td>
      <td>{input_dataset}</td>
      <td>log_20230126</td>
      <td>2023-01-27</td>
      <td>CREATE TABLE `{input_project_id}.{input_dataset}.log_2023...</td>
      <td>CREATE TABLE IF NOT EXISTS `{input_project_id}.{input_data...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>{input_project_id}</td>
      <td>{input_dataset}</td>
      <td>log_20230127</td>
      <td>2023-01-27</td>
      <td>CREATE TABLE `{input_project_id}.{input_dataset}.log_2023...</td>
      <td>CREATE TABLE IF NOT EXISTS `{input_project_id}.{input_data...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>{input_project_id}</td>
      <td>{input_dataset}</td>
      <td>appsflyer_20220617</td>
      <td>2023-01-27</td>
      <td>CREATE TABLE `{input_project_id}.{input_dataset}.appsflyer_2...</td>
      <td>CREATE TABLE IF NOT EXISTS `{input_project_id}.{input_data...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>{input_project_id}</td>
      <td>{input_dataset}</td>
      <td>appsflyer_20220618</td>
      <td>2023-01-27</td>
      <td>CREATE TABLE `{input_project_id}.{input_dataset}.appsflyer_2...</td>
      <td>CREATE TABLE IF NOT EXISTS `{input_project_id}.{input_data...</td>
    </tr>
  </tbody>
</table>
</div>



