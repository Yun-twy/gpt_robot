# Project schedule

다음의 목록을 가능한 만큼 애자일 방식으로 개발해 나갈 생각이다.
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
5. fine-tuning시킨 ChatGPT 모델 탑재(해당 노드는 비용 문제로 필요할 때만 활성화시킬 것)  
6. 여러 스케쥴 목록이 들어올 경우 하나씩 수행하는 동작
7. 스케쥴 수행중 필요시 ChatGPT 모델에 현재 상황, 시행중인 스케쥴, 상태를 알려주고 스케쥴을 정정해줄 것을 요구하는 부분
8. 시연 퍼포먼스 향상을 위해 추가적으로 있어야 할 기능들 구현
    - 웹크롤링을 통한 필요한 정보 GPT 모델에 제공
    - TTS 모델을 이용 ChatGPT가 하고 싶은 말을 출력
    - 맵 파일 위에 검은 안개 레이어를 추가로 씌우고(정찰시마다 초기화), 안개가 일정 비율 이상 거칠때까지 맵을 돌아다니는 정찰 기능
   - 지나가던 중 물건이 보이면 맵상 좌표에 해당 물건을 기록해두고, 필요할 경우 해당 위치로 이동, 없을 경우 정찰하게하는 코드
9. 하드웨어 교체
   - 로봇팔 제어
   - 뎁스카메라를 이용 정확한 물체 매핑
---
### 사전 준비
- 원래 [Deepspeech stt 모델](https://github.com/sooftware/kospeech)을 이용하여 학습해 사용하려 했으나, 컴퓨팅 파워 부족으로 [이미 학습되어 있는 딥러닝 모델](https://github.com/kakaobrain/pororo)을 사용하기로 결정. 성능은 양호해보인다. 
- ChatGPT 사전 리서치 : 비용문제 때문에 연습용으로는 curie 모델을 fine-tuning해서 사용하고, 이후 시연시 davich 모델을 fine-tuning해서 사용하게 될 것으로 생각됨. 이를 위한 fine-tune 데이터셋은 어느정도 제작해둠(시나리오 추가 요구)
- TTS 모델은 눈여겨봐둔 모델이 존재하나 다른 모델을 사용하게 될 것으로 생각됨 

### 2023.5.30
- [stt 활용](https://github.com/sooftware/kospeech#introduction) --> whisper 사용하여 녹음파일 텍스트화 성공!

- [audioROS](https://github.com/LCAV/audioROS) 패키지를 활용해서 오디오 퍼블리셔, 서브스크라이버 제작. 일정 시간 퍼블리셔가 구동하다 정지하는 현상 해결
  하지만, 음성이 뚝뚝 끊기는 문제 발생. ROS통신의 속도에 한계가 존재해서 딜레이를 청크단위로 끊어서 그런 것으로 생각됨.
  이 문제를 해결하기 위해선 2가지 솔루션이 존재할 것으로 예상됨
  - 1. 한번에 보내는 사이즈를 키워서 슬로우 모션으로 들리는 상태로 STT 모델에 건네주는 것 (이 경우 느린 음원을 STT 모델이 제대로 알아들을까 미지수)
  - 2. 서브스크라이버 측에서 짧은 시간동안 청크를 모아서 짧은 음원을 만들고 STT 모델에 넣어 1,2 글자를 뱉게 해서 이것을 하나의 문장으로 모으는 것
    (주로 [realtime STT](https://github.com/davabase/whisper_real_time/blob/master/transcribe_demo.py)가 이런 방식을 채택하는 것으로 보임.) 
