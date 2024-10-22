---
{"dg-publish":true,"permalink":"/object-detection/model/bug-report/"}
---

2024.10.22(Tue) rt-DETR 결과 분석
![업로드한 이미지](https://files.oaiusercontent.com/file-vKUSkO3c71mbOzyZVuWVV72H?se=2024-10-22T02%3A58%3A44Z&sp=r&sv=2024-08-04&sr=b&rscc=max-age%3D299%2C%20immutable%2C%20private&rscd=attachment%3B%20filename%3D%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202024-10-22%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB%252011.53.16.png&sig=ZWQ/ZrF0uD3ZBBEpYT6NBXJ8A8XYdApYNe3lPAf%2B%2Bdc%3D)
클래스 라벨이 하나씩 밀린 현상이 발생했다.
가설1: 학습 및 변환 과정에서 클래스 ID와 실제 클래스 간의 매핑이 잘못되어 발생했을 것이다.
가설2: category 문제가 아니고 .txt로 변환하는 과정에서 json을 잘못 읽어온 거라면?

가설 1 solution
- `convert_yolo` 함수 수정
	`category_id`를 조정하는 부분에서 클래스 ID가 정확히 일치하도록 수정한다. 
	if COCO 형식의 `category_id`가 이미 0부터 시작한다면, `category_id -= 1`을 하면 클래스 매핑이 밀린다.

가설 2 solution
- ID 오프셋 확인: `category_id`에서 1을 빼서 YOLO 형식의 클래스 ID로 변환할 때, 1을 빼지 않아야 할 경우 혹은 데이터셋에 따라 조정이 필요한 경우가 있습니다. 예를 들어, `category_id`가 이미 0부터 시작한다면 1을 빼지 않아야한다.