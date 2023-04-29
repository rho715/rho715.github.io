---
layout: single
title: "Airflow start_date 정리"
categories: ['Airflow']
tag: []
toc: false
author_profile: false
sidebar:
    nav: "docs"
search: true
---

## Airflow의 실행 여부 차이
작업 배포 시간: `2023-04-29 16:10:00 (KST)`

1. `v1.0.1` 
    ```python
    import os
    from airflow import DAG
    from airflow.operators.dummy import DummyOperator
    from airflow.operators.python import PythonOperator

    from datetime import datetime, timedelta
    import pytz
    import pendulum

    def print_time(**kwargs) -> str:
        time_utc = datetime.now()
        time_kst = time_utc + timedelta(hours=9)
        logical_date = kwargs.get('logical_date')

        print("UTC time: ", time_utc)
        print("KST time: ", time_kst)
        print("context logical_date: ", logical_date)
        print("context logical_date (kst): ", logical_date + timedelta(hours=9))
        return

    kst_timezone = pytz.timezone('Asia/Seoul')

    OWNER = 'rho715@'
    DAG_ID = os.path.basename(__file__).replace(".pyc", "").replace(".py", "")
    DAG_NAME = f'prd01_{DAG_ID}_v1.0.1'
    default_args = {
        'owner': OWNER,
        'dag_id': DAG_ID,
        'depends_on_past': False,
        'start_date': pendulum.datetime(2023, 4, 28, 15, 0, 0,  tz='Asia/Seoul'), #pendulum.now(tz='Asia/Seoul') - timedelta(days=1),
        'email_on_failure': False,
        'email_on_retry': False,
    }

    with DAG(DAG_NAME,
            default_args=default_args,
            dagrun_timeout=timedelta(hours=2),
            max_active_runs=1,
            max_active_tasks=1,
            catchup=False,
            is_paused_upon_creation=True,
            schedule_interval='00 15 * * *',
            tags=['testing']
    ) as dag:
        start = DummyOperator(
            task_id='start',
            dag=dag
        )

        task_01 = PythonOperator(
            task_id='task_01',
            python_callable=print_time,
            provide_context=True,
            execution_timeout=timedelta(minutes=30),
            dag=dag
        )

        end = DummyOperator(
            task_id='end',
            dag=dag
        )

        start >> task_01 >> end
    ```

2. `v1.0.2`

    ```python
    import os
    from airflow import DAG
    from airflow.operators.dummy import DummyOperator
    from airflow.operators.python import PythonOperator

    from datetime import datetime, timedelta
    import pytz
    import pendulum

    def print_time(**kwargs) -> str:
        time_utc = datetime.now()
        time_kst = time_utc + timedelta(hours=9)
        logical_date = kwargs.get('logical_date')

        print("UTC time: ", time_utc)
        print("KST time: ", time_kst)
        print("context logical_date: ", logical_date)
        print("context logical_date (kst): ", logical_date + timedelta(hours=9))

        return

    kst_timezone = pytz.timezone('Asia/Seoul')

    OWNER = 'rho715@'
    DAG_ID = os.path.basename(__file__).replace(".pyc", "").replace(".py", "")
    DAG_NAME = f'prd01_{DAG_ID}_v1.0.2'
    default_args = {
        'owner': OWNER,
        'dag_id': DAG_ID,
        'depends_on_past': False,
        'start_date': pendulum.datetime(2023, 4, 28, 17, 0, 0,  tz='Asia/Seoul'), #pendulum.now(tz='Asia/Seoul') - timedelta(days=1),
        'email_on_failure': False,
        'email_on_retry': False,
    }

    with DAG(DAG_NAME,
            default_args=default_args,
            dagrun_timeout=timedelta(hours=2),
            max_active_runs=1,
            max_active_tasks=1,
            catchup=False,
            is_paused_upon_creation=True,
            schedule_interval='00 17 * * *',
            tags=['testing']
    ) as dag:
        start = DummyOperator(
            task_id='start',
            dag=dag
        )

        task_01 = PythonOperator(
            task_id='task_01',
            python_callable=print_time,
            provide_context=True,
            execution_timeout=timedelta(minutes=30),
            dag=dag
        )

        end = DummyOperator(
            task_id='end',
            dag=dag
        )

        start >> task_01 >> end
    ```


## 결과 
   <div style="display: flex;">
        <img src="{{site.url}}/images/2023-04-29/airflow_dag.png" alt="Airflow" style="width: 90%;">
    </div>

### `v1.0.1`은 실행이 되고 `v1.0.2`는 실행이 안된 이유

- 구글에서 찾은 [블로그](https://blog.bsk.im/2021/03/21/apache-airflow-aip-39/)에 의하면 airflow는 time window라는 개념을 이해해야한다고 함. 쇼핑몰을 좋은 예시로 이야기하고 있는데 요약하자면,
- 2023년 4월 28일 쇼핑몰 신규 가입자가 몇 명인지 집계하고자 한다.
    - 신규 가입자 대상: 23년 4월 28일 00~24시 사이에 쇼핑몰에 가입한 이용자
- 상기 조건에 따라 집계하고자 하는 가입자들의 날짜는 4월 28일이지만 취합은 28일 24시를 지난 4월 29일에 진행해야함.

시간까지 예시로 들어보자. 

- `v1.0.1`은 `start_date`를 23년 4월 28일 오후 3시로 등록 & `schedule_interval`도 매일 오후 3시로 설정. 이 뜻은 결국 23년 4월 28일 오후 3시의 데이터를 23년 4월 29일 오후 3시에 취합 작업을 실행한다는 뜻. 내가 작업을 23년 4월 29일 오후 4시에 등록했으니 4월 29일 3시에 돌았어야할 작업이 돌지 않아서 작업을 자동으로 실행해 주었다. 

- `v1.0.2`는 `start_date`를 23년 4월 28일 오후 5시로 등록 & `schedule_interval`도 매일 오후 5시로 설정. 따라서 23년 4월 29일 오후 5시에 작업이 실행되는데. 내가 작업을 23년 4월 29일 오후 4시에 등록했으니 5시가 되면 자동으로 실행될 예정이여서 돌지 않았다. 

어떻게 보면 당연한 이야기 같지만 실제로 이러한 로직을 모르면 언제는 실행이 되고 언제는 실행이 안되는지 헷갈린다. 업무에서 Airflow를 사용하면서 가장 헷갈렸던 것은 매시 돌아가는 배치 스케줄을 등록했을때이다. 

- 회사에서는 보통 아래와 같이 설정하고 있는데
    ```python
    OWNER = 'rho715@'
    DAG_ID = os.path.basename(__file__).replace(".pyc", "").replace(".py", "")
    DAG_NAME = f'prd01_{DAG_ID}_v1.0.2'
    default_args = {
        'owner': OWNER,
        'dag_id': DAG_ID,
        'depends_on_past': False,
        'start_date': pendulum.now(tz='Asia/Seoul') - timedelta(days=1),
        'email_on_failure': False,
        'email_on_retry': False,
    }

    with DAG(DAG_NAME,
            default_args=default_args,
            dagrun_timeout=timedelta(hours=2),
            max_active_runs=1,
            max_active_tasks=1,
            catchup=False,
            is_paused_upon_creation=True,
            schedule_interval="10 * * * *",
            tags=['testing']
            ) as dag:
        start = DummyOperator(
            task_id='start',
            dag=dag
        )

        task_01 = PythonOperator(
            task_id='task_01',
            python_callable=print_time,
            provide_context=True,
            execution_timeout=timedelta(minutes=30),
            dag=dag
        )

        end = DummyOperator(
            task_id='end',
            dag=dag
        )

        start >> task_01 >> end
    ```
    `catchup` 변수를 `False`로 지정했는데도 불구하고 왜 실행이 될까 항상 의아했는데 아래와 같이 설정해줘야하는 부분을 이번에 알게 되었다
        
    - AS-IS: `'start_date': pendulum.now(tz='Asia/Seoul') - timedelta(days=1)` 
    - TO-BE: `'start_date': pendulum.now(tz='Asia/Seoul') - timedelta(hours=1)`
    - 추가로 매시 10분 작업 스케줄을 등록했으면 00~10분사이에 작업이 airflow에 등록 되어야 내가 원하는 시간대부터 스케줄이 돌아갔다. 예를 들어 
        - 23년 4월 29일 오후 5시 5분에 매시 10분마다 실행되는 DAG를 등록할 경우,
            - `logical_date: 2023-04-29 16:10:00` 작업이 `2023-04-29 17:10:00`에 실행이 된다 
        - 하지만 23년 4월 29일 오후 5시 15분에 매시 10분마다 실행되는 DAG를 등록할 경우,
            - `logical_date: 2023-04-29 17:10:00` 작업이 `2023-04-29 18:10:00`에 실행이 된다고 기대할 수 있지만 
            - 실제로는  `logical_date: 2023-04-29 16:10:00` 작업이 `2023-04-29 17:15:00`에 실행이 된다.

catchup & backfill도 매우 중요한 개념이니 짚고 넘어가자 [블로그](https://magpienote.tistory.com/236)

