---
layout: single
title: "Cloud Logging Search"
categories: [GCP]
tag: [Cloud Logging]
toc: false
sidebar:
    nav: "docs"
search: true
---

## Cloud Logging에서 빠르게 로그찾기 [link](https://cloud.google.com/vertex-ai/docs/workbench/managed/audit-logging#api)

### Vertex AI - Create Instance 
```python
resource.labels.method="google.cloud.notebooks.v1.NotebookService.CreateInstance"
resource.labels.project_id="{project_id}"
resource.labels.service="notebooks.googleapis.com"
resource.type="audited_resource"
protoPayload.request.@type="type.googleapis.com/google.cloud.notebooks.v1.CreateInstanceRequest"
severity>=NOTICE
```

### Vertex AI - Delete Instance
```python
resource.labels.method="google.cloud.notebooks.v1.NotebookService.DeleteInstance"
resource.labels.project_id="{project_id}"
resource.labels.service="notebooks.googleapis.com"
resource.type="audited_resource"
protoPayload.request.@type="type.googleapis.com/google.cloud.notebooks.v1.DeleteInstanceRequest"
severity>=NOTICE
```

### Composer - Create 
```python
logName="projects/{project_id}/logs/cloudaudit.googleapis.com%2Factivity"
resource.labels.project_id="{project_id}"
resource.type="cloud_composer_environment"
protoPayload.@type="type.googleapis.com/google.cloud.audit.AuditLog"
protoPayload.serviceName="composer.googleapis.com"
protoPayload.authorizationInfo.permission="composer.environments.create"
severity>=NOTICE
```

### Composer - Delete 
```python
logName="projects/{project_id}/logs/cloudaudit.googleapis.com%2Factivity"
resource.labels.project_id="{project_id}"
resource.type="cloud_composer_environment"
protoPayload.@type="type.googleapis.com/google.cloud.audit.AuditLog"
protoPayload.serviceName="composer.googleapis.com"
protoPayload.authorizationInfo.permission="composer.environments.delete"
severity>=NOTICE
```