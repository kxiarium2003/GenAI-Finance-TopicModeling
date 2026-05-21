# GenAI-Finance-TopicModeling

2026-1 Text Mining Individual Assignment  
생성형 AI(Generative AI)와 금융 산업(Finance/Banking/FinTech) 관련 연구 문헌을 수집한 뒤, BERTopic 기반 토픽 모델링을 수행하여 주요 연구 주제를 분석한 프로젝트입니다.

본 프로젝트에서는 Web of Science(WOS)에서 수집한 논문의 Abstract를 전처리한 후 BERTopic을 활용해 주요 연구 흐름을 도출하였습니다.  
이후 생성형 AI(ChatGPT)를 활용한 토픽 분석 결과와 비교 분석을 진행하였으며, 해당 비교 내용은 최종 보고서에 기술하였습니다.

---

## Project Overview

- 주제: **Generative AI and Changes in the Financial Industry**
- 데이터 출처: Web of Science (WOS)
- 검색 Query:

```text
("generative AI" OR "large language model" OR "LLM" OR "ChatGPT")
AND
(finance OR banking OR fintech)
```
- 수집 논문 수: 602 papers (12 papers before 2023, 590 papers from 2023–2026)
- 분석 대상: 논문 Abstract

## Topic Modeling Pipeline

본 프로젝트는 BERTopic 기반 토픽 모델링 파이프라인을 사용하였습니다.

#### Pipeline
1. Abstract 전처리
2. SentenceTransformer (all-MiniLM-L6-v2) 기반 문장 임베딩
3. UMAP 차원 축소
4. HDBSCAN 클러스터링
5. c-TF-IDF 기반 토픽 키워드 추출
6. BERTopic 토픽 생성 및 시각화

## Environments

| Notebook                  | Environment         |
| ------------------------- | ------------------- |
| 01_preprocessing_I.ipynb  | Local (VS Code)     |
| 02_BERTopic_I.ipynb       | Google Colab T4 GPU |
| 03_preprocessing_II.ipynb | Google Colab T4 GPU |
| 04_BERTopic_II.ipynb      | Google Colab T4 GPU |

## Directory Structure
```
GenAI-Finance-TopicModeling/
│
├── WOS_data.xls
│
├── preprocessing/
│   ├── 01_preprocessing_I.ipynb      # Local VS Code, 1st preprocessing
│   └── 03_preprocessing_II.ipynb     # Colab T4 GPU, 2nd preprocessing
│
├── BERTopic/
│   ├── 02_BERTopic_I.ipynb           # 1st BERTopic modeling
│   └── 04_BERTopic_II.ipynb          # 2nd BERTopic modeling
│
├── outputs/
│   ├── output_01_cleaned_abstracts.csv
│   ├── output_02_cleaned_abstracts.csv
│   └── output_03_topic_info.csv
│
├── plots/
│   ├── plot01_Intertopic_Distance_Map.png
│   ├── plot02_Topic_Word_Scores.png
│   └── plot03_Topic_Similarity_Matrix.png
│
└── README.md
```

## Output Files
| File                              | Description           |
| --------------------------------- | --------------------- |
| `output_01_cleaned_abstracts.csv` | 1차 전처리 결과             |
| `output_02_cleaned_abstracts.csv` | 2차 전처리 결과             |
| `output_03_topic_info.csv`        | 최종 BERTopic 토픽 모델링 결과 |

## Preprocessing

전처리 과정에서는 다음 작업을 수행하였습니다.

- 소문자 변환
- 특수문자 제거
- 숫자 제거
- 불용어 제거
- 표제어 추출 (Lemmatization)
- Domain-specific stopwords 제거

특히 다음과 같은 과도하게 빈번한 단어들은 토픽 분류의 의미를 약화시킬 수 있어 custom stopwords로 제거하였습니다.
```
model, llm, research, study, method,
paper, result, approach, using, based ...
```
## BERTopic Settings

주요 BERTopic 설정은 다음과 같습니다.

#### Embedding Model
```
SentenceTransformer("all-MiniLM-L6-v2")
```
#### UMAP
```
n_neighbors=15
n_components=5
metric='cosine'
```
#### HDBSCAN
```
min_cluster_size=15
cluster_selection_method='eom'
```
#### CountVectorizer
```
ngram_range=(1, 2)
min_df=5
```
## Major Topics Identified

예시 토픽:
- Financial Sentiment Analysis
- Privacy & Security Risks
- Blockchain & DeFi
- Retrieval-Augmented Generation (RAG)
- Bias & Fairness in Financial LLMs
- Hallucination and Reliability 

### Notes
- 본 레포지토리에는 BERTopic 기반 토픽 모델링 과정만 포함되어 있습니다.
- ChatGPT 기반 토픽 비교 분석 내용은 최종 보고서에 포함되어 있습니다.