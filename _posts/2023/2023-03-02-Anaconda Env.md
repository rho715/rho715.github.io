---
layout: single
title: "Anaconda Environment"
categories: [Anaconda3]
tag: [Python]
toc: true
toc_sticky: true
toc_icon: "fas fa-sign"
sidebar:
    nav: "docs"
search: true
---

## conda env

### environment 생성하기
```console
conda create -n {env_name} python=3.#
``` 

### environment 리스트 보기 
```console
conda env list
``` 

### environment 활성화 
```console
conda activate {env_name}
``` 

### environment 비활성화
```console
conda deactivate {env_name}
``` 

### environment 삭제
```console
conda env remove -n {env_name}
``` 

### environment 클론
```console
conda create -n {new_env} --clone {org_env}
``` 

### environment freeze 
```console
pip freeze > requirements.txt
```

### install requirements.txt
```console
pip install -r requirements.txt
``` 

### uninstall requirements.xt 
```console
pip uninstall -r  requirements.txt
``` 


## packages 
### get installed packages list
```console
conda list 
``` 

### install a package
```
conda install pandas 
``` 

### install packages 
```console
conda install pandas numpy 
``` 

### update a package 
```console
conda update pandas 
``` 

### update a package 
```console
conda upgrade --all 
``` 

### delete a package 
```console
conda remove pandas 
``` 

## 출처 
- [https://teddylee777.github.io/python/anaconda-%EA%B0%80%EC%83%81%ED%99%98%EA%B2%BD%EC%84%A4%EC%A0%95-%ED%8C%81-%EA%B0%95%EC%A2%8C/](https://teddylee777.github.io/python/anaconda-%EA%B0%80%EC%83%81%ED%99%98%EA%B2%BD%EC%84%A4%EC%A0%95-%ED%8C%81-%EA%B0%95%EC%A2%8C/)
- [https://yganalyst.github.io/basic/anaconda_env_1/](https://yganalyst.github.io/basic/anaconda_env_1/)