---
layout: single
title: "BigQuery Sharded Tables"
categories: [BigQuery, SQL]
tag: [sharded, yyyymmdd, offset, regex]
toc: false
author_profile: false
sidebar:
    nav: "docs"
search: true
---

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


