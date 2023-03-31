---
layout: single
title: "Azure OpenAI"
categories: [AI]
tag: [Azure, OpenAI, chatGPT]
toc: false
author_profile: false
toc: true
toc_sticky: true
toc_icon: "fas fa-sign"
sidebar:
    nav: "docs"
search: true
---

# 리서치 내용 정리 
- Azure OpenAI Service
    - 사용자가 OpenAI 모델을 사용하여 엔터프라이즈급 솔루션을 빌드 할 수 있다
    - 텍스트를 요약하고, 코드를 제안 받고, 웹사이트에 대한 이미지를 생성하는 등 작업이 가능하다
    - 주요 기능
        - **자연어 생성 → 복잡한 텍스트 요약, 문장의 대체 표현 제안 등**
        - **코드 생성 → python → Go로 변환, 코드 버그 식별, 문제 해결 등**
        - **이미지 생성 → 텍스트 설명에서 발행물에 대한 이미지 생성**
- chatGPT와 같이 소프트웨어 애플리케이션을 구축할 수 있도록 AI모델을 제공한다
- 종류
    - **인공 지능**은 출력할 내용에 대한 명시적 지침 없이 머신에 의존하여 작업을 학습하고 실행함으로써 인간의 행동을 모방합니다.
    - **기계 학습** 모델은 날씨 조건과 같은 데이터를 가져와서 알고리즘에 데이터를 맞추어 매장이 하루에 얼마나 많은 돈을 벌 수 있는지 예측합니다.
    - **딥 러닝** 모델은 인공 신경망 형태의 알고리즘 계층을 사용하여 더 복잡한 사용 사례에 대한 결과를 반환합니다. Azure AI 서비스는 딥 러닝 모델을 기반으로 합니다. [기계 학습과 딥 러닝의 차이점](https://learn.microsoft.com/ko-kr/azure/machine-learning/concept-deep-learning-vs-machine-learning)에 대해 자세히 알아보려면 이 문서를 참조하세요.
        - 기계학습 vs 딥러닝
    - **생성 AI** 모델은 입력에 설명된 내용을 기반으로 새 콘텐츠를 생성할 수 있는 딥 러닝 모델의 하위 집합입니다. OpenAI 모델은 언어, 코드, 이미지를 생성할 수 있는 생성 AI 모델의 컬렉션입니다.
    
    ![generative-ai](https://learn.microsoft.com/ko-kr/training/wwl-data-ai/explore-azure-openai/media/generative-ai.png)
- Azure OpenAI는 Azure 사용자가 사용할 수 있으며 다음 네 가지 구성 요소로 구성됩니다.
    - 미리 학습된 생성 AI 모델
    - 사용자 지정 기능 - 사용자 고유의 데이터로 AI 모델을 미세 조정할 수 있는 기능
    - 사용자가 AI를 책임감 있게 구현할 수 있도록 유해한 사용 사례를 감지하고 완화하는 기본 제공 도구
    - RBAC(역할 기반 액세스 제어) 및 개인 네트워크를 사용하는 엔터프라이즈급 보안
- OpenAI 서비스 간에는 **번역, 감정 분석, 키워드 추출** 등 여러 가지 겹치는 기능이 있습니다.
- AI 활용기능
    - **기계 학습** - 이것은 종종 AI 시스템의 기초이며 예측하고 데이터에서 결론을 도출하기 위해 컴퓨터 모델을 "가르치는" 방법입니다.
    - **변칙 검색** - 시스템에서 오류 또는 비정상적인 활동을 자동으로 감지할 수 있는 기능입니다.
    - **Computer Vision** - 카메라, 비디오 및 이미지를 통해 세계를 시각적으로 해석할 수 있는 소프트웨어 기능입니다.
    - **자연어 처리** - 컴퓨터가 서면 또는 음성 언어를 해석하고 동일하게 응답할 수 있는 기능입니다.
    - **지식 마이닝** - 종종 대용량의 비정형 데이터에서 정보를 추출하여 검색 가능한 지식 저장소를 만드는 기능입니다.
- 중요성
    - 공정성
    - 신뢰성 및 안정성
    - 개인 정보 보호 및 보안
    - 포용성
    - 투명성
    - 책임성

# Approach
- AI 활용기능 중에서 자연어 처리 & 지식 마이닝 위주로 테스트 하고자 함
    - **기계 학습** - 이것은 종종 AI 시스템의 기초이며 예측하고 데이터에서 결론을 도출하기 위해 컴퓨터 모델을 "가르치는" 방법입니다.
    - **변칙 검색** - 시스템에서 오류 또는 비정상적인 활동을 자동으로 감지할 수 있는 기능입니다.
    - **Computer Vision** - 카메라, 비디오 및 이미지를 통해 세계를 시각적으로 해석할 수 있는 소프트웨어 기능입니다.
    - **자연어 처리 - 컴퓨터가 서면 또는 음성 언어를 해석하고 동일하게 응답할 수 있는 기능입니다.**
    - **지식 마이닝 - 종종 대용량의 비정형 데이터에서 정보를 추출하여 검색 가능한 지식 저장소를 만드는 기능입니다.**

# 실제 신청한 Application 기능
   <div style="display: flex;">
        <img src="{{site.url}}/images/2023-03-31/Screenshot 2023-03-28 at 2.26.35 PM.png" alt="Azure OpenAI" style="width: 70%;">
    </div>

# 참고한 자료
- 적용 사례
    - 자막 번역
        <div style="display: flex;">
            <img src="{{site.url}}/images/2023-03-31/1680089457988.jpg" alt="auto-sub-1" style="width: 50%;">
            <img src="{{site.url}}/images/2023-03-31/1680089458113.jpg" alt="auto-sub-2" style="width: 50%;">
        </div>
        
        [[한글자막] Big Ideas 2023  Artificial Intelligence Creating the Assembly Line for Knowledge Workers](https://www.youtube.com/watch?v=HZupD6To1Ek)
        
    - [마이리얼트립](https://yozm.wishket.com/magazine/detail/1949/)
        - MyrealGPT (Itinerary Creator)
        - “ 지금 당장 중요한 것, 구현해야 하는것, 지금 당장 필요한가”
        - “어떻게 사용하는지에 관한 경험과 정보가 없는 이들도 어떻게해야 chatGPT를 자연스럽게 사용할 수 있을까”
        - 세밀한 UI
    - [서울디지털재단 - ChatGPT 활용사례 및 활용 팁](https://sdf.seoul.kr/research-report/2003?curPage=1)
        - chatGPT 3
            - 자연어 이해의 정확성이 매우 높고, 맥락을 이해하여 연결 질문 가능하며, 파라미터 값을 조합하여 결과를 출력
            - 21년 10월까지 데이터에 한정되어 검색
            - 파라미터 값을 조합하는 과정에서 거짓말을 만들어 내서 답하는 경우도 종종 있음 (전문가 주장과 같이 출처가 필요한 내용에 대해서는 활용 지양)
            - 활용 분야
                - 업무
                    - 보고서 자료 조사
                    - 사업기획 아이디어
                    - 글쓰기, 보도 자료, 번역 및 교정
                    - 엑셀
                    - 프로그래밍
                - 일상
                    - 자문 (법률, 투자, 건강, 심리, 진로, 정비, 영어공부-영어교사처럼 행동하게 하는 명령어를 입력하여 대회 및 즉시 교정이 가능하다)
                - 창작
                    - 블로그 및 글쓰기
                    - 노래 가사 & 시 & 소설
                    - 유튜브 스크립트 대본 등
            - 활용성 강화 팁
                - 하이퍼파라미터 설정하기 (max_length: 2048, writing style: Journalistic, tone: humorous)
                - 크롬 확장 프로그램
                    - WebChatGPT (최근 자료 사용 가능)
                    - promptGenie? (즉시 번역 가능)
                    - AIPRM for chatGPT (더 정확한 결과값을 위한 양식)
                    - Chat for Google (구글 검색시 연동)
                - chatGPT 잘쓰는 방법을 chatGPT한테도 물어볼 수 있다
    - pdf 학습 → 요약 및 질문 ([chatpdf.com](https://www.chatpdf.com/))