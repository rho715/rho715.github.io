---
layout: single
title: "Airflow Config"
categories: [Airflow]
tag: [context, kwargs]
toc: false
author_profile: false
sidebar:
    nav: "docs"
search: true
---

#### Airflow Dag python file

[참고](https://pinggoopark.tistory.com/682)

```python
# file name : dag_config.py

#############################  LIBRARIES
from airflow import DAG
from airflow.operators.python_operator import PythonOperator
from datetime import datetime
import pprint

dag = DAG (
    dag_id = 'Airflow_config',
    start_date = datetime(2022, 12, 3),
    schedule_interval = "@daily",
    catchup = False,
    tags = ['log_test'],
    description = 'Python Operator Sample',
    default_args = {'owner': 'rhorho'})

def func_print_context(**context):
    return pprint.pprint(context)

def func_print_kwargs(**kwargs):
    return pprint.pprint(kwargs)

task_print_context = PythonOperator(task_id = 'print_context', python_callable = func_print_context, dag = dag, do_xcom_push = False)
task_print_kwargs = PythonOperator(task_id = 'print_kwargs', python_callable = func_print_kwargs, dag = dag, do_xcom_push = False)

task_print_context >> task_print_kwargs
```

##### context output

```python
*** Reading local file: /opt/airflow/logs/Airflow_config/print_context/2023-01-29T00:00:00+00:00/1.log
[2023-01-30, 08:23:21 UTC] {taskinstance.py:1043} INFO - Dependencies all met for <TaskInstance: Airflow_config.print_context scheduled__2023-01-29T00:00:00+00:00 [queued]>
[2023-01-30, 08:23:21 UTC] {taskinstance.py:1043} INFO - Dependencies all met for <TaskInstance: Airflow_config.print_context scheduled__2023-01-29T00:00:00+00:00 [queued]>
[2023-01-30, 08:23:21 UTC] {taskinstance.py:1249} INFO - 
--------------------------------------------------------------------------------
[2023-01-30, 08:23:21 UTC] {taskinstance.py:1250} INFO - Starting attempt 1 of 1
[2023-01-30, 08:23:21 UTC] {taskinstance.py:1251} INFO - 
--------------------------------------------------------------------------------
[2023-01-30, 08:23:21 UTC] {taskinstance.py:1270} INFO - Executing <Task(PythonOperator): print_context> on 2023-01-29 00:00:00+00:00
[2023-01-30, 08:23:21 UTC] {standard_task_runner.py:52} INFO - Started process 12973 to run task
[2023-01-30, 08:23:21 UTC] {standard_task_runner.py:79} INFO - Running: ['***', 'tasks', 'run', 'Airflow_config', 'print_context', 'scheduled__2023-01-29T00:00:00+00:00', '--job-id', '312', '--raw', '--subdir', 'DAGS_FOLDER/sample/dag_config.py', '--cfg-path', '/tmp/tmp9uhulypx', '--error-file', '/tmp/tmp_nxk2dcq']
[2023-01-30, 08:23:21 UTC] {standard_task_runner.py:80} INFO - Job 312: Subtask print_context
[2023-01-30, 08:23:21 UTC] {logging_mixin.py:109} INFO - Running <TaskInstance: Airflow_config.print_context scheduled__2023-01-29T00:00:00+00:00 [running]> on host 69279047b7b3
[2023-01-30, 08:23:21 UTC] {taskinstance.py:1448} INFO - Exporting the following env vars:
AIRFLOW_CTX_DAG_OWNER=rhorho
AIRFLOW_CTX_DAG_ID=Airflow_config
AIRFLOW_CTX_TASK_ID=print_context
AIRFLOW_CTX_EXECUTION_DATE=2023-01-29T00:00:00+00:00
AIRFLOW_CTX_DAG_RUN_ID=scheduled__2023-01-29T00:00:00+00:00
[2023-01-30, 08:23:21 UTC] {warnings.py:110} WARNING - /home/***/.local/lib/python3.7/site-packages/***/utils/context.py:220: AirflowContextDeprecationWarning: Accessing 'execution_date' from the template is deprecated and will be removed in a future version. Please use 'data_interval_start' or 'logical_date' instead.
  warnings.warn(_create_deprecation_warning(k, replacements))

[2023-01-30, 08:23:21 UTC] {warnings.py:110} WARNING - /home/***/.local/lib/python3.7/site-packages/***/utils/context.py:220: AirflowContextDeprecationWarning: Accessing 'next_ds' from the template is deprecated and will be removed in a future version. Please use '{{ data_interval_end | ds }}' instead.
  warnings.warn(_create_deprecation_warning(k, replacements))

[2023-01-30, 08:23:21 UTC] {warnings.py:110} WARNING - /home/***/.local/lib/python3.7/site-packages/***/utils/context.py:220: AirflowContextDeprecationWarning: Accessing 'next_ds_nodash' from the template is deprecated and will be removed in a future version. Please use '{{ data_interval_end | ds_nodash }}' instead.
  warnings.warn(_create_deprecation_warning(k, replacements))

[2023-01-30, 08:23:21 UTC] {warnings.py:110} WARNING - /home/***/.local/lib/python3.7/site-packages/***/utils/context.py:220: AirflowContextDeprecationWarning: Accessing 'next_execution_date' from the template is deprecated and will be removed in a future version. Please use 'data_interval_end' instead.
  warnings.warn(_create_deprecation_warning(k, replacements))

[2023-01-30, 08:23:21 UTC] {warnings.py:110} WARNING - /home/***/.local/lib/python3.7/site-packages/***/utils/context.py:220: AirflowContextDeprecationWarning: Accessing 'prev_ds' from the template is deprecated and will be removed in a future version.
  warnings.warn(_create_deprecation_warning(k, replacements))

[2023-01-30, 08:23:21 UTC] {warnings.py:110} WARNING - /home/***/.local/lib/python3.7/site-packages/***/utils/context.py:220: AirflowContextDeprecationWarning: Accessing 'prev_ds_nodash' from the template is deprecated and will be removed in a future version.
  warnings.warn(_create_deprecation_warning(k, replacements))

[2023-01-30, 08:23:21 UTC] {warnings.py:110} WARNING - /home/***/.local/lib/python3.7/site-packages/***/utils/context.py:220: AirflowContextDeprecationWarning: Accessing 'prev_execution_date' from the template is deprecated and will be removed in a future version.
  warnings.warn(_create_deprecation_warning(k, replacements))

[2023-01-30, 08:23:21 UTC] {warnings.py:110} WARNING - /home/***/.local/lib/python3.7/site-packages/***/utils/context.py:220: AirflowContextDeprecationWarning: Accessing 'prev_execution_date_success' from the template is deprecated and will be removed in a future version. Please use 'prev_data_interval_start_success' instead.
  warnings.warn(_create_deprecation_warning(k, replacements))

[2023-01-30, 08:23:21 UTC] {warnings.py:110} WARNING - /home/***/.local/lib/python3.7/site-packages/***/utils/context.py:220: AirflowContextDeprecationWarning: Accessing 'tomorrow_ds' from the template is deprecated and will be removed in a future version.
  warnings.warn(_create_deprecation_warning(k, replacements))

[2023-01-30, 08:23:21 UTC] {warnings.py:110} WARNING - /home/***/.local/lib/python3.7/site-packages/***/utils/context.py:220: AirflowContextDeprecationWarning: Accessing 'tomorrow_ds_nodash' from the template is deprecated and will be removed in a future version.
  warnings.warn(_create_deprecation_warning(k, replacements))

[2023-01-30, 08:23:21 UTC] {warnings.py:110} WARNING - /home/***/.local/lib/python3.7/site-packages/***/utils/context.py:220: AirflowContextDeprecationWarning: Accessing 'yesterday_ds' from the template is deprecated and will be removed in a future version.
  warnings.warn(_create_deprecation_warning(k, replacements))

[2023-01-30, 08:23:21 UTC] {warnings.py:110} WARNING - /home/***/.local/lib/python3.7/site-packages/***/utils/context.py:220: AirflowContextDeprecationWarning: Accessing 'yesterday_ds_nodash' from the template is deprecated and will be removed in a future version.
  warnings.warn(_create_deprecation_warning(k, replacements))

[2023-01-30, 08:23:21 UTC] {logging_mixin.py:109} INFO - {'conf': <***.configuration.AirflowConfigParser object at 0x400999f490>,
 'conn': None,
 'dag': <DAG: Airflow_config>,
 'dag_run': <DagRun Airflow_config @ 2023-01-29 00:00:00+00:00: scheduled__2023-01-29T00:00:00+00:00, externally triggered: False>,
 'data_interval_end': DateTime(2023, 1, 30, 0, 0, 0, tzinfo=Timezone('UTC')),
 'data_interval_start': DateTime(2023, 1, 29, 0, 0, 0, tzinfo=Timezone('UTC')),
 'ds': '2023-01-29',
 'ds_nodash': '20230129',
 'execution_date': <Proxy at 0x4022685320 with factory functools.partial(<function lazy_mapping_from_context.<locals>._deprecated_proxy_factory at 0x40225ca7a0>, 'execution_date', DateTime(2023, 1, 29, 0, 0, 0, tzinfo=Timezone('UTC')))>,
 'inlets': [],
 'logical_date': DateTime(2023, 1, 29, 0, 0, 0, tzinfo=Timezone('UTC')),
 'macros': <module '***.macros' from '/home/***/.local/lib/python3.7/site-packages/***/macros/__init__.py'>,
 'next_ds': <Proxy at 0x4022686320 with factory functools.partial(<function lazy_mapping_from_context.<locals>._deprecated_proxy_factory at 0x40225ca7a0>, 'next_ds', '2023-01-30')>,
 'next_ds_nodash': <Proxy at 0x4022686460 with factory functools.partial(<function lazy_mapping_from_context.<locals>._deprecated_proxy_factory at 0x40225ca7a0>, 'next_ds_nodash', '20230130')>,
 'next_execution_date': <Proxy at 0x4022686af0 with factory functools.partial(<function lazy_mapping_from_context.<locals>._deprecated_proxy_factory at 0x40225ca7a0>, 'next_execution_date', DateTime(2023, 1, 30, 0, 0, 0, tzinfo=Timezone('UTC')))>,
 'outlets': [],
 'params': {},
 'prev_data_interval_end_success': None,
 'prev_data_interval_start_success': None,
 'prev_ds': <Proxy at 0x4022669780 with factory functools.partial(<function lazy_mapping_from_context.<locals>._deprecated_proxy_factory at 0x40225ca7a0>, 'prev_ds', '2023-01-28')>,
 'prev_ds_nodash': <Proxy at 0x40226812d0 with factory functools.partial(<function lazy_mapping_from_context.<locals>._deprecated_proxy_factory at 0x40225ca7a0>, 'prev_ds_nodash', '20230128')>,
 'prev_execution_date': <Proxy at 0x4022681550 with factory functools.partial(<function lazy_mapping_from_context.<locals>._deprecated_proxy_factory at 0x40225ca7a0>, 'prev_execution_date', datetime.datetime(2023, 1, 28, 0, 0, tzinfo=Timezone('UTC')))>,
 'prev_execution_date_success': <Proxy at 0x40226815f0 with factory functools.partial(<function lazy_mapping_from_context.<locals>._deprecated_proxy_factory at 0x40225ca7a0>, 'prev_execution_date_success', None)>,
 'prev_start_date_success': None,
 'run_id': 'scheduled__2023-01-29T00:00:00+00:00',
 'task': <Task(PythonOperator): print_context>,
 'task_instance': <TaskInstance: Airflow_config.print_context scheduled__2023-01-29T00:00:00+00:00 [running]>,
 'task_instance_key_str': 'Airflow_config__print_context__20230129',
 'templates_dict': None,
 'test_mode': False,
 'ti': <TaskInstance: Airflow_config.print_context scheduled__2023-01-29T00:00:00+00:00 [running]>,
 'tomorrow_ds': <Proxy at 0x4022681050 with factory functools.partial(<function lazy_mapping_from_context.<locals>._deprecated_proxy_factory at 0x40225ca7a0>, 'tomorrow_ds', '2023-01-30')>,
 'tomorrow_ds_nodash': <Proxy at 0x402268ff00 with factory functools.partial(<function lazy_mapping_from_context.<locals>._deprecated_proxy_factory at 0x40225ca7a0>, 'tomorrow_ds_nodash', '20230130')>,
 'ts': '2023-01-29T00:00:00+00:00',
 'ts_nodash': '20230129T000000',
 'ts_nodash_with_tz': '20230129T000000+0000',
 'var': {'json': None, 'value': None},
 'yesterday_ds': <Proxy at 0x402268f1e0 with factory functools.partial(<function lazy_mapping_from_context.<locals>._deprecated_proxy_factory at 0x40225ca7a0>, 'yesterday_ds', '2023-01-28')>,
 'yesterday_ds_nodash': <Proxy at 0x402268f280 with factory functools.partial(<function lazy_mapping_from_context.<locals>._deprecated_proxy_factory at 0x40225ca7a0>, 'yesterday_ds_nodash', '20230128')>}
[2023-01-30, 08:23:21 UTC] {python.py:175} INFO - Done. Returned value was: None
[2023-01-30, 08:23:22 UTC] {taskinstance.py:1288} INFO - Marking task as SUCCESS. dag_id=Airflow_config, task_id=print_context, execution_date=20230129T000000, start_date=20230130T082321, end_date=20230130T082322
[2023-01-30, 08:23:22 UTC] {local_task_job.py:154} INFO - Task exited with return code 0
[2023-01-30, 08:23:22 UTC] {local_task_job.py:264} INFO - 1 downstream tasks scheduled from follow-on schedule check

```

##### kwargs output

```python
*** Reading local file: /opt/airflow/logs/Airflow_config/print_kwargs/2023-01-29T00:00:00+00:00/1.log
[2023-01-30, 08:23:24 UTC] {taskinstance.py:1043} INFO - Dependencies all met for <TaskInstance: Airflow_config.print_kwargs scheduled__2023-01-29T00:00:00+00:00 [queued]>
[2023-01-30, 08:23:24 UTC] {taskinstance.py:1043} INFO - Dependencies all met for <TaskInstance: Airflow_config.print_kwargs scheduled__2023-01-29T00:00:00+00:00 [queued]>
[2023-01-30, 08:23:24 UTC] {taskinstance.py:1249} INFO - 
--------------------------------------------------------------------------------
[2023-01-30, 08:23:24 UTC] {taskinstance.py:1250} INFO - Starting attempt 1 of 1
[2023-01-30, 08:23:24 UTC] {taskinstance.py:1251} INFO - 
--------------------------------------------------------------------------------
[2023-01-30, 08:23:24 UTC] {taskinstance.py:1270} INFO - Executing <Task(PythonOperator): print_kwargs> on 2023-01-29 00:00:00+00:00
[2023-01-30, 08:23:24 UTC] {standard_task_runner.py:52} INFO - Started process 12988 to run task
[2023-01-30, 08:23:24 UTC] {standard_task_runner.py:79} INFO - Running: ['***', 'tasks', 'run', 'Airflow_config', 'print_kwargs', 'scheduled__2023-01-29T00:00:00+00:00', '--job-id', '313', '--raw', '--subdir', 'DAGS_FOLDER/sample/dag_config.py', '--cfg-path', '/tmp/tmpvdh3b0h1', '--error-file', '/tmp/tmpqte2bfrv']
[2023-01-30, 08:23:24 UTC] {standard_task_runner.py:80} INFO - Job 313: Subtask print_kwargs
[2023-01-30, 08:23:24 UTC] {logging_mixin.py:109} INFO - Running <TaskInstance: Airflow_config.print_kwargs scheduled__2023-01-29T00:00:00+00:00 [running]> on host 69279047b7b3
[2023-01-30, 08:23:24 UTC] {taskinstance.py:1448} INFO - Exporting the following env vars:
AIRFLOW_CTX_DAG_OWNER=rhorho
AIRFLOW_CTX_DAG_ID=Airflow_config
AIRFLOW_CTX_TASK_ID=print_kwargs
AIRFLOW_CTX_EXECUTION_DATE=2023-01-29T00:00:00+00:00
AIRFLOW_CTX_DAG_RUN_ID=scheduled__2023-01-29T00:00:00+00:00
[2023-01-30, 08:23:24 UTC] {warnings.py:110} WARNING - /home/***/.local/lib/python3.7/site-packages/***/utils/context.py:220: AirflowContextDeprecationWarning: Accessing 'execution_date' from the template is deprecated and will be removed in a future version. Please use 'data_interval_start' or 'logical_date' instead.
  warnings.warn(_create_deprecation_warning(k, replacements))

[2023-01-30, 08:23:24 UTC] {warnings.py:110} WARNING - /home/***/.local/lib/python3.7/site-packages/***/utils/context.py:220: AirflowContextDeprecationWarning: Accessing 'next_ds' from the template is deprecated and will be removed in a future version. Please use '{{ data_interval_end | ds }}' instead.
  warnings.warn(_create_deprecation_warning(k, replacements))

[2023-01-30, 08:23:24 UTC] {warnings.py:110} WARNING - /home/***/.local/lib/python3.7/site-packages/***/utils/context.py:220: AirflowContextDeprecationWarning: Accessing 'next_ds_nodash' from the template is deprecated and will be removed in a future version. Please use '{{ data_interval_end | ds_nodash }}' instead.
  warnings.warn(_create_deprecation_warning(k, replacements))

[2023-01-30, 08:23:24 UTC] {warnings.py:110} WARNING - /home/***/.local/lib/python3.7/site-packages/***/utils/context.py:220: AirflowContextDeprecationWarning: Accessing 'next_execution_date' from the template is deprecated and will be removed in a future version. Please use 'data_interval_end' instead.
  warnings.warn(_create_deprecation_warning(k, replacements))

[2023-01-30, 08:23:24 UTC] {warnings.py:110} WARNING - /home/***/.local/lib/python3.7/site-packages/***/utils/context.py:220: AirflowContextDeprecationWarning: Accessing 'prev_ds' from the template is deprecated and will be removed in a future version.
  warnings.warn(_create_deprecation_warning(k, replacements))

[2023-01-30, 08:23:24 UTC] {warnings.py:110} WARNING - /home/***/.local/lib/python3.7/site-packages/***/utils/context.py:220: AirflowContextDeprecationWarning: Accessing 'prev_ds_nodash' from the template is deprecated and will be removed in a future version.
  warnings.warn(_create_deprecation_warning(k, replacements))

[2023-01-30, 08:23:24 UTC] {warnings.py:110} WARNING - /home/***/.local/lib/python3.7/site-packages/***/utils/context.py:220: AirflowContextDeprecationWarning: Accessing 'prev_execution_date' from the template is deprecated and will be removed in a future version.
  warnings.warn(_create_deprecation_warning(k, replacements))

[2023-01-30, 08:23:24 UTC] {warnings.py:110} WARNING - /home/***/.local/lib/python3.7/site-packages/***/utils/context.py:220: AirflowContextDeprecationWarning: Accessing 'prev_execution_date_success' from the template is deprecated and will be removed in a future version. Please use 'prev_data_interval_start_success' instead.
  warnings.warn(_create_deprecation_warning(k, replacements))

[2023-01-30, 08:23:24 UTC] {warnings.py:110} WARNING - /home/***/.local/lib/python3.7/site-packages/***/utils/context.py:220: AirflowContextDeprecationWarning: Accessing 'tomorrow_ds' from the template is deprecated and will be removed in a future version.
  warnings.warn(_create_deprecation_warning(k, replacements))

[2023-01-30, 08:23:24 UTC] {warnings.py:110} WARNING - /home/***/.local/lib/python3.7/site-packages/***/utils/context.py:220: AirflowContextDeprecationWarning: Accessing 'tomorrow_ds_nodash' from the template is deprecated and will be removed in a future version.
  warnings.warn(_create_deprecation_warning(k, replacements))

[2023-01-30, 08:23:24 UTC] {warnings.py:110} WARNING - /home/***/.local/lib/python3.7/site-packages/***/utils/context.py:220: AirflowContextDeprecationWarning: Accessing 'yesterday_ds' from the template is deprecated and will be removed in a future version.
  warnings.warn(_create_deprecation_warning(k, replacements))

[2023-01-30, 08:23:24 UTC] {warnings.py:110} WARNING - /home/***/.local/lib/python3.7/site-packages/***/utils/context.py:220: AirflowContextDeprecationWarning: Accessing 'yesterday_ds_nodash' from the template is deprecated and will be removed in a future version.
  warnings.warn(_create_deprecation_warning(k, replacements))

[2023-01-30, 08:23:24 UTC] {logging_mixin.py:109} INFO - {'conf': <***.configuration.AirflowConfigParser object at 0x400999f490>,
 'conn': None,
 'dag': <DAG: Airflow_config>,
 'dag_run': <DagRun Airflow_config @ 2023-01-29 00:00:00+00:00: scheduled__2023-01-29T00:00:00+00:00, externally triggered: False>,
 'data_interval_end': DateTime(2023, 1, 30, 0, 0, 0, tzinfo=Timezone('UTC')),
 'data_interval_start': DateTime(2023, 1, 29, 0, 0, 0, tzinfo=Timezone('UTC')),
 'ds': '2023-01-29',
 'ds_nodash': '20230129',
 'execution_date': <Proxy at 0x4022a85500 with factory functools.partial(<function lazy_mapping_from_context.<locals>._deprecated_proxy_factory at 0x40225cdcb0>, 'execution_date', DateTime(2023, 1, 29, 0, 0, 0, tzinfo=Timezone('UTC')))>,
 'inlets': [],
 'logical_date': DateTime(2023, 1, 29, 0, 0, 0, tzinfo=Timezone('UTC')),
 'macros': <module '***.macros' from '/home/***/.local/lib/python3.7/site-packages/***/macros/__init__.py'>,
 'next_ds': <Proxy at 0x4022a85960 with factory functools.partial(<function lazy_mapping_from_context.<locals>._deprecated_proxy_factory at 0x40225cdcb0>, 'next_ds', '2023-01-30')>,
 'next_ds_nodash': <Proxy at 0x4022a850f0 with factory functools.partial(<function lazy_mapping_from_context.<locals>._deprecated_proxy_factory at 0x40225cdcb0>, 'next_ds_nodash', '20230130')>,
 'next_execution_date': <Proxy at 0x4022a85870 with factory functools.partial(<function lazy_mapping_from_context.<locals>._deprecated_proxy_factory at 0x40225cdcb0>, 'next_execution_date', DateTime(2023, 1, 30, 0, 0, 0, tzinfo=Timezone('UTC')))>,
 'outlets': [],
 'params': {},
 'prev_data_interval_end_success': None,
 'prev_data_interval_start_success': None,
 'prev_ds': <Proxy at 0x4022929730 with factory functools.partial(<function lazy_mapping_from_context.<locals>._deprecated_proxy_factory at 0x40225cdcb0>, 'prev_ds', '2023-01-28')>,
 'prev_ds_nodash': <Proxy at 0x4022a7d5f0 with factory functools.partial(<function lazy_mapping_from_context.<locals>._deprecated_proxy_factory at 0x40225cdcb0>, 'prev_ds_nodash', '20230128')>,
 'prev_execution_date': <Proxy at 0x4022a7dcd0 with factory functools.partial(<function lazy_mapping_from_context.<locals>._deprecated_proxy_factory at 0x40225cdcb0>, 'prev_execution_date', datetime.datetime(2023, 1, 28, 0, 0, tzinfo=Timezone('UTC')))>,
 'prev_execution_date_success': <Proxy at 0x4022a8e370 with factory functools.partial(<function lazy_mapping_from_context.<locals>._deprecated_proxy_factory at 0x40225cdcb0>, 'prev_execution_date_success', None)>,
 'prev_start_date_success': None,
 'run_id': 'scheduled__2023-01-29T00:00:00+00:00',
 'task': <Task(PythonOperator): print_kwargs>,
 'task_instance': <TaskInstance: Airflow_config.print_kwargs scheduled__2023-01-29T00:00:00+00:00 [running]>,
 'task_instance_key_str': 'Airflow_config__print_kwargs__20230129',
 'templates_dict': None,
 'test_mode': False,
 'ti': <TaskInstance: Airflow_config.print_kwargs scheduled__2023-01-29T00:00:00+00:00 [running]>,
 'tomorrow_ds': <Proxy at 0x4022a8eb90 with factory functools.partial(<function lazy_mapping_from_context.<locals>._deprecated_proxy_factory at 0x40225cdcb0>, 'tomorrow_ds', '2023-01-30')>,
 'tomorrow_ds_nodash': <Proxy at 0x4022a8eb40 with factory functools.partial(<function lazy_mapping_from_context.<locals>._deprecated_proxy_factory at 0x40225cdcb0>, 'tomorrow_ds_nodash', '20230130')>,
 'ts': '2023-01-29T00:00:00+00:00',
 'ts_nodash': '20230129T000000',
 'ts_nodash_with_tz': '20230129T000000+0000',
 'var': {'json': None, 'value': None},
 'yesterday_ds': <Proxy at 0x4022a8e0f0 with factory functools.partial(<function lazy_mapping_from_context.<locals>._deprecated_proxy_factory at 0x40225cdcb0>, 'yesterday_ds', '2023-01-28')>,
 'yesterday_ds_nodash': <Proxy at 0x4022a8efa0 with factory functools.partial(<function lazy_mapping_from_context.<locals>._deprecated_proxy_factory at 0x40225cdcb0>, 'yesterday_ds_nodash', '20230128')>}
[2023-01-30, 08:23:24 UTC] {python.py:175} INFO - Done. Returned value was: None
[2023-01-30, 08:23:24 UTC] {taskinstance.py:1288} INFO - Marking task as SUCCESS. dag_id=Airflow_config, task_id=print_kwargs, execution_date=20230129T000000, start_date=20230130T082324, end_date=20230130T082324
[2023-01-30, 08:23:25 UTC] {local_task_job.py:154} INFO - Task exited with return code 0
[2023-01-30, 08:23:25 UTC] {local_task_job.py:264} INFO - 0 downstream tasks scheduled from follow-on schedule check

```