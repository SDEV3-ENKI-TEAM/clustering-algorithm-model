# clustering-algorithm-model
using k-means algorithm / elbow method / silhouette score

## k-means algorithm
엘보 기법을 이용하여 팔처럼 굽어지는 부분을 k값으로 지정
실루엣 기법을 사용 -> 하나의 데이터 객체가 다른 클러스터에 비해 자신이 속한 클러스터와 얼마나 유사한지 측정
-1 ~ +1 까지 있으며, 값이 높을수록 클러스터링이 잘 된것
목표치 : 0.7 이상



<img width="841" height="547" alt="image" src="https://github.com/user-attachments/assets/4b91b5b5-d824-4bde-a7db-272f33050ce9" />
<img width="855" height="547" alt="image" src="https://github.com/user-attachments/assets/caa272d2-881c-40ad-ba19-daf4f26eb16b" />

k=4로 선택할경우 (trace.zip 대상)
<img width="1012" height="701" alt="image" src="https://github.com/user-attachments/assets/5251dded-db88-49dc-8e3d-0810ddf76849" />
주요 행위 / 부수 행위 정도에 따라 악성 행위 안에서도 어떤 악성 행위인지 클러스터링

### 추가적으로 작업
- 휴리스틱 룰 기반 하드코딩 추가
- 데이터 노이즈 제거 (토큰 수 제한)
- 청크 효과적으로 분리
- 임베딩 모델 -> 더 나은거 찾아보기


#### 프롬프트 개선
rag + cot(생각의 사슬) 를 혼합하여 프롬프트의 질을 높이고, llm 출력 성능을 향상하고자 함
ex>
ai_prompt = f"""
    당신은 최고의 사이버 보안 분석 전문가입니다. 알고리즘에 의해 '하나의 그룹'으로 분류된 최대 5개의 로그 요약문을 제공합니다.

    당신은 다음의 "단계별 사고 과정(Chain of Thought)"을 반드시 따라야 합니다:

    1.  **[분석]:** 먼저, 제공된 각 요약문을 읽고 공통적으로 나타나는 핵심 행위, 프로세스 이름, 의심스러운 TTPs (공격 기술, 전술, 절차)를 식별합니다. 발견한 공통 패턴을 요약합니다.
    2.  **[분류]:** 당신의 [분석] 내용을 바탕으로, 이 클러스터가 다음 중 어떤 특정 공격 유형일 가능성이 가장 높은지 하나만 선택하십시오: (Ransomware, Credential Dumping, Defense Evasion, Persistence(지속성 유지), Worm, Trojan, C2 Communication)
    3.  **[결론]:** 왜 그렇게 [분류]했는지, [분석] 단계에서 찾은 핵심 증거를 인용하여 한국어로 간결하게 정당화하십시오.

    아래 제공된 [출력 형식]을 정확히 지켜서 응답하십시오.

    ---[출력 형식]---
    **[분석 (Chain of Thought)]:**
    (여기에 공통 패턴 및 TTPs 분석 내용을 서술)

    **[분류 (Classification)]:**
    (여기에 선택한 단일 공격 유형을 서술)

    **[결론 (Justification)]:**
    (여기에 분류에 대한 핵심 근거를 서술)
    ---[끝] ---


    --- [제공된 요약문 (RAG Context)] ---
    {prompt_summaries}
    --- [요약문 끝] ---
    """

기존 유클리드 거리 계산법에서 -> 코사인 거리 계산법으로 변경
클러스터 별 대표 요약값 출력시 -> 각 클러스터의 대표 요약값 5개를 묶어서 llm에게 질문을 던지고 / 어떤 세부 공격인지 예측함 (ex> worm일 가능성이 높다, trojan일 가능성이 높다, etc....)


#### 추가
실루엣 스코어 계수가 0.5에서 머무름
-> 악성공격 안에서도 세부적으로 나누는 과정에서 낮아진것?
-> 자연어 변환 후 임베딩하는 것보단 우선 자연어 변환 전 전처리 데이터를 임베딩하고 클러스터링 알고리즘 적용시 실루엣 스코어 계수가 0.67로 높아진 것을 확인
-> 고려 사항... 자연어로도 실루엣 스코어를 높혀놔야함
