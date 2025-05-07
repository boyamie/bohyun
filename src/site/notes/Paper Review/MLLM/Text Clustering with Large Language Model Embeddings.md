---
{"dg-publish":true,"permalink":"/paper-review/mllm/text-clustering-with-large-language-model-embeddings/"}
---

2024년에 아카이브에 제출된 논문이다.
먼저 텍스트 클러스터링의 정의에 대해 알아보자.
text clustering은 NLP의 기본적인 과제로, 문서들을 의미 있는 그룹으로 묶어야 한다.
하지만 언어의 복잡성 때문에 기존의 클러스터링 방식은 한계가 있다.

### Need
텍스트 데이터를 조직화해서 RAG에서 retriveval 효율을 향상시킬 수 있다.

### How
서로 다른 텍스트는 다른 클러스터에 그룹핑해야하는데 어떻게 하는걸까?
텍스트를 표현하는 고차원의 vector를 계산해야한다.
고차원 벡터공간의 축은 텍스트의 frequency 또는 embedding 기법을 통해 특징들이 추출된 feature space다.
클러스터링 알고리즘은 feature space의 proximity, resemblance, correspondence 방법론을 사용한다.

LLM을 직접 클러스터링에 사용하는 것이 아니라
guide로 활용해서 기존 embedder 기반 클러스터링을 강화한다.

# Triplet Task
triplet task를 통해서 임베더를 개선해서 임베딩 품질을 향상시킨다.
triplet 구성: anchor문장, 후보 문장1, 후보 문장2 -> LLM이 후보 중에 어떤 문장이 anchor와 유사한지 판단

CLUSTERLLM: 엔트로피 높은 데이터만 선택: 각 데이터 포인트의 클러스터 확률 분포 계산 -> 엔트로피가 높은 데이터를 anchor로 선택 -> 서로 다른 클러스터에서 후보 문장 선택 -> 이 triplet을 LLM에게 질의
