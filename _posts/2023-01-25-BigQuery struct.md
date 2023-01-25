---
layout: single
title: "BigQuery Struct 구조 정리하기"
categories: coding
tag: [BigQuery, struct(), Array]
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

Array
```sql
     SELECT
     -- {project_id}
          -- {dataset1}
          ['{project_id}.{dataset1}.{datatable_names}'
          , '{project_id}.{dataset1}.{datatable_names}'
          , '{project_id}.{dataset1}.{datatable_names}'
          , '{project_id}.{dataset1}.{datatable_names}'
          -- {dataset2}
          , '{project_id}.{dataset2}.{datatable_names}'
          , '{project_id}.{dataset2}.{datatable_names}'
          , '{project_id}.{dataset2}.{datatable_names}'
          , '{project_id}.{dataset2}.{datatable_names}'
          , '{project_id}.{dataset2}.{datatable_names}'
          , '{project_id}.{dataset2}.{datatable_names}'
          , '{project_id}.{dataset2}.{datatable_names}'
          -- {dataset3} 
          , '{project_id}.{dataset3}.{datatable_names}'
          , '{project_id}.{dataset3}.{datatable_names}'
          , '{project_id}.{dataset3}.{datatable_names}'
          , '{project_id}.{dataset3}.{datatable_names}'
          -- {dataset4}
          , '{project_id}.{dataset4}.{datatable_names}' ] as table_list
```

Struct
```sql
     SELECT
     -- {project_id}
          -- {dataset1}
          [struct('{project_id}.{dataset1}.{datatable_names}' as table_address, 'DAG' as gen_type, 'a' as gen_link, 'b' as regen_link)
          , struct('{project_id}.{dataset1}.{datatable_names}' as table_address, 'DAG' as gen_type, 'a' as gen_link, 'b' as regen_link)
          , struct('{project_id}.{dataset1}.{datatable_names}' as table_address, 'DAG' as gen_type, 'a' as gen_link, 'b' as regen_link)
          , struct('{project_id}.{dataset1}.{datatable_names}' as table_address, 'DAG' as gen_type, 'a' as gen_link, 'b' as regen_link)
          -- {dataset2}
          , struct('{project_id}.{dataset2}.{datatable_names}' as table_address, 'DAG' as gen_type, 'a' as gen_link, 'b' as regen_link)
          , struct('{project_id}.{dataset2}.{datatable_names}' as table_address, 'DAG' as gen_type, 'a' as gen_link, 'b' as regen_link)
          , struct('{project_id}.{dataset2}.{datatable_names}' as table_address, 'DAG' as gen_type, 'a' as gen_link, 'b' as regen_link)
          , struct('{project_id}.{dataset2}.{datatable_names}' as table_address, 'DAG' as gen_type, 'a' as gen_link, 'b' as regen_link)
          , struct('{project_id}.{dataset2}.{datatable_names}' as table_address, 'DAG' as gen_type, 'a' as gen_link, 'b' as regen_link)
          , struct('{project_id}.{dataset2}.{datatable_names}' as table_address, 'DAG' as gen_type, 'a' as gen_link, 'b' as regen_link)
          , struct('{project_id}.{dataset2}.{datatable_names}' as table_address, 'DAG' as gen_type, 'a' as gen_link, 'b' as regen_link)
          -- {dataset3} 
          , struct('{project_id}.{dataset3}.{datatable_names}' as table_address, 'DAG' as gen_type, 'a' as gen_link, 'b' as regen_link)
          , struct('{project_id}.{dataset3}.{datatable_names}' as table_address, 'DAG' as gen_type, 'a' as gen_link, 'b' as regen_link)
          , struct('{project_id}.{dataset3}.{datatable_names}' as table_address, 'DAG' as gen_type, 'a' as gen_link, 'b' as regen_link)
          , struct('{project_id}.{dataset3}.{datatable_names}' as table_address, 'DAG' as gen_type, 'a' as gen_link, 'b' as regen_link)
          -- {dataset4}
          , struct('{project_id}.{dataset4}.{datatable_names}' as table_address, 'DAG' as gen_type, 'a' as gen_link, 'b' as regen_link) ] as meta
```

[참고링크](https://medium.com/google-cloud/how-to-work-with-array-and-structs-in-bigquery-9c0a2ea584a6)


Use [dag_run.conf](https://dydwnsekd.tistory.com/64)
BashOperator
```python
# file name : rho_test_dag_config.py

#############################  LIBRARIES
import sys
import os
from datetime import datetime, timedelta, timezone
from google.cloud import bigquery
import json

from airflow import DAG
from airflow.operators.python_operator import PythonOperator
from airflow.operators.bash import BashOperator
from airflow.models import Variable
from airflow.utils.dates import days_ago

from dag_utils.slack_cloudfn import SlackAlert
#############################  MODULE PATH
def get_path(_path, step, _dir=None):
    up_path = os.sep.join(_path.split(os.sep)[:-step])
    if _dir is None:
        return up_path
    return os.path.join(up_path, _dir)

module_path = get_path(os.path.dirname(os.path.abspath(__file__)), 1)
sys.path.append(module_path)


# ############################################################################################################################### common #####
default_args= {
    'start_date': days_ago(1),
    'retries': 0,
    'catchup': False,
    'retry_delay': timedelta(minutes=5),
}

templated_command = """
    echo "ds : {{ dag_run.conf }}"
    """

dag = DAG(
    'templated_test',
    default_args=default_args,
    schedule_interval="@daily",
)

t1 = BashOperator(
    task_id='bash_templated',
    bash_command=templated_command,
    dag=dag,
)

t1
```

pythonOperator
```python
# file name : rho_test_dag_config.py

#############################  LIBRARIES
import sys
import os
from datetime import datetime, timedelta, timezone
from google.cloud import bigquery
import json

from airflow import DAG
from airflow.operators.python_operator import PythonOperator
from airflow.operators.bash import BashOperator
from airflow.models import Variable
from airflow.utils.dates import days_ago

from dag_utils.slack_cloudfn import SlackAlert
#############################  MODULE PATH
def get_path(_path, step, _dir=None):
    up_path = os.sep.join(_path.split(os.sep)[:-step])
    if _dir is None:
        return up_path
    return os.path.join(up_path, _dir)

module_path = get_path(os.path.dirname(os.path.abspath(__file__)), 1)
sys.path.append(module_path)


# ############################################################################################################################### common #####
default_args = {
    'start_date': datetime.now() - timedelta(days=1),
    'retries': 0,
    'catchup': False,
    'retry_delay': timedelta(minutes=5),
}

def print_ds_conf(**kwargs):
    conf = kwargs['dag_run'].conf
    if conf == {} :
        print("conf is empty")
    else :
        print("ds : ", conf)

dag = DAG(
    'templated_test_python',
    default_args=default_args,
    schedule_interval="@daily",
)

t1 = PythonOperator(
    task_id='python_templated_python',
    python_callable=print_ds_conf,
    provide_context=True,
    dag=dag,
)

t1
```