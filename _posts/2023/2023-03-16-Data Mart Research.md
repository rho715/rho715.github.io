---
layout: single
title: "Data Mart Research"
categories: [GCP, Airflow]
tag: [Data Mart, BigQuery]
toc: false
author_profile: false
sidebar:
    nav: "docs"
search: true
---

## Data Mart Research
- 내가 생각하는 Data Mart:
    <div class="notice--success">
    <h4>
    Data Warehouse 및 외부데이터를 일정의 로직으로 정규화 & 규칙적으로 생성하여 보다 효율적이고 빠른 비즈니스 인사이트를 도출하는데 도움을 줄 수 있는 데이터 
    </h4>
    </div>

- (왼: [Oracle](https://www.oracle.com/kr/autonomous-database/what-is-data-mart/); 오: [AWS](https://aws.amazon.com/ko/what-is/data-mart/))
    <div style="display: flex;">

        <img src="{{site.url}}/images/2023-03-16/DataMart - Oracle.png" alt="chatGPT" style="width: 50%;">
        <img src="{{site.url}}/images/2023-03-16/DataMart - AWS.png" alt="GCP-monitoring" style="width: 50%;">
    </div>

- 키워드 
    - 요약
    - 신속
    - 간소화
    - 분석/비즈니스 인사이트 
    - 부서 특화
    - 효율 검색
    - 세분화된 권한관리 및 정보 제어
    - 유연한 데이터 관리 (용량)

- 작업 단계
    - 원시 정보를 비즈니스 부서의 분석을 위해 정형 콘텐츠로 변환
    - 데이터 엔지니어는 웨어하우스/외부 소스에서 불필요한 정보를 필터링 필요한 정보만 집계하여 제공 

- 종류
    - **종속 데이터 마트**
        - 데이터 웨어하우스에서 주제별 정보를 쿼리하고 검색
        - 장점: 대부분의 데이터 관리 작업은 데이터 웨어하우스 단계에서 수행
        - 단점: 단일 장애 지점 (데이터 웨어하우스에 장애가 있으면 데이터마트도 실패) 
    - 독립 데이터 마트
        - 중앙 데이터 웨어하우스나 다른 데이터 마트에 의존하지 않음 
        - 장점: 쉽게 구축
        - 단점: 관리가 어려움
    - 하이브리드 데이터 마트 


## Application
- 내가 회사에서 관리하고있는 데이터 마트의 종류는 **종속 데이터 마트**로 데이터 웨어하우스 데이터 장애에 따라 영향도 및 의존성이 매우 높음 
- 즉 데이터 마트가 잘 생성이 되려면 데이터 웨어하우스의 데이터가 잘 들어왔는지 확인해야함 
- 데이터 웨어하우스가 잘 들어왔는지 체크하고 데이터 웨어하우스가 오류가 났을 때 어떻게 데이터를 보다 빠르게 재생성 할 수 있는지를.. 찾고싶은데 왜 블로그 글은 없을까...
- 다들.. 데이터 마트가 잘 생성되나... :'(
- 여튼 나는 그래서 어찌하려는가!!

## 결국 간단히 정리하면 
1. 빅쿼리 테이블 관리겸 standard SQL을 통해 meta 정보를 조회하여 각 테이블별로 `last_modified_date` 불러오도록 작업 (`__TABLES__`)
2. 데이터마트 테이블별로 참조하는 data warehouse 테이블 리스트를 추출
3. 추출된 데이터 소스와 1번에서 추출한 마지막 수정일을 조인하여 만일 data warehouse 테이블이 최신화가 안되어있다면 slack 채널에 알람이 오도록 설정 함 
