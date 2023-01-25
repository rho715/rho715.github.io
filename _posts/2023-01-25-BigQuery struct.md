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

1. Array
```SQL
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

2. Struct
```SQL
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