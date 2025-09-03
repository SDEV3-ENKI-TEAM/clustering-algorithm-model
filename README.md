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
