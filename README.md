# Project Goal

1. audio publish & subscribe
2. 음성으로 조작되는 로봇
3. 기본적인 로봇 기능 구현
    - 전후 좌우 이동(맵상에 좌표찍기로)
    - 좌우회전 / 후진
    - LED 양쪽 각각 제어
    - 부저 제어
    - 특정 지점으로 이동(길찾기 실패해서 벽쳐다보고 버벅이지 않게) 
    - 아르코마커를 보고 현재 위치 정정
4. YOLO 모델(COCO Dataset) 탑재를 통한 물체 시각적 인식 
    - 해당 물체를 바라보기, 물체를 향해서 이동, 물체를 일정시간 따라가기 등..
6. fine-tuning시킨 ChatGPT 모델 탑재(해당 노드는 비용 문제로 필요할 때만 활성화시킬 것)  
7. 여러 스케쥴 목록이 들어올 경우 하나씩 수행하는 동작
8. 스케쥴 수행중 필요시 ChatGPT 모델에 현재 상황, 시행중인 스케쥴, 상태를 알려주고 스케쥴을 정정해줄 것을 요구하는 부분
9. 시연 퍼포먼스 향상을 위해 추가적으로 있어야 할 기능들 구현
    - 웹크롤링을 통한 필요한 정보 GPT 모델에 제공
    - TTS 모델을 이용 ChatGPT가 하고 싶은 말을 출력
    - 맵 파일 위에 검은 안개 레이어를 추가로 씌우고(정찰시마다 초기화), 안개가 일정 비율 이상 거칠때까지 맵을 돌아다니는 정찰 기능
   - 지나가던 중 물건이 보이면 맵상 좌표에 해당 물건을 기록해두고, 필요할 경우 해당 위치로 이동, 없을 경우 정찰하게하는 코드
10. 하드웨어 교체
   - 로봇팔 제어
   - 뎁스카메라를 이용 정확한 물체 매핑
---
## 팀원 소개
- [송승훈](https://github.com/addinedu-amr-2th/robot-repo-4/tree/ssh)
- [윤태웅](https://github.com/addinedu-amr-2th/robot-repo-4/tree/ytw)

