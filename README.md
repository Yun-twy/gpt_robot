## Project Intro
![image](https://github.com/addinedu-amr-2th/robot-repo-4/assets/69943723/90dd1ab6-1b3f-4c26-9b3a-ec2bda80d337)

거대 언어 모델 **ChatGPT**를 이용한 로봇 행동 제어 프로젝트  
## 시연 영상
https://github.com/addinedu-amr-2th/robot-repo-4/assets/69943723/4dd73bd7-c1d2-4d25-b1f7-6d03d8f8011c

## 목적
- AGI로도 생각되는 **거대 언어 모델(LLM)인 ChatGPT**를 사용하여 사용자의 요청을 해결하는 **로봇의 적절한 논리적인 행동리스트를 스스로 판단, 생성**시켜보자.
- 대화와 감정을 만들어 낼 수 있는 ChatGPT의 특성을 강조하여 인간과 로봇의 교감에 초점을 맞춘다.
- 기존에 없던 **ChatGPT를 이용해 로봇을 제어하는 새로운 프레임 워크** 개발
## 프로젝트 설명  
![image](https://github.com/addinedu-amr-2th/robot-repo-4/assets/69943723/26047294-6db2-4bc7-9b68-9fade1467827)

- 전체 시스템 구성도 : 사용자의 음성을 언어모델인 ChatGPT에 입력시키기 위해 google STT API를 사용, finetuning된 GPT 모델에 사용자의 요청에 해당하는 프롬프트를 넣고, 그 출력으로 나온 일련의 동작 시퀀스인 스케쥴을 스케쥴러에서 하나씩 parsing하여 여러 하드웨어들을 제어할 수 있도록 한다.
---
![image](https://github.com/addinedu-amr-2th/robot-repo-4/assets/69943723/0a197b3a-89fc-4266-a0ff-da29f2f745bd)

- finetuning : OpenAI에서 제공하는 서비스를 통해 입력 `prompt`와 모범답안 `completion`의 한 쌍으로 GPT 모델을 finetuning시켜 학습데이터의 말투, 문법, 스타일을 사용하는 나만의 GPT 모델을 만들 수 있다.
- 우리는 일련의 행동을 언어로 규정하기 위해서 기기, 행동, 파라미터를 콜론(:) 마크로, 동작과 동작 단위를 별(*) 마크로 정의하여 이를 기준으로 출력 스케쥴을 parsing할 수 있었다.
---
![image](https://github.com/addinedu-amr-2th/robot-repo-4/assets/69943723/17dad57b-8b71-414a-920a-39f0fede7a58)

- 우리는 최적의 성능을 얻기 위해 위와 같은 기법들을 사용했다.
- 왼쪽(ChatGPT)과 오른쪽(GPT3)을 따로 놓은 이유는 OpenAI에서 제공하는 엔드포인트가 다르고, 사용하는 데이터 형태도 다르며, 모델의 성향조차 다르기 때문이다.
- ChatGPT의 경우 `system`을 통해 역할을 부여하면 이 역할을 지키려하는 역할극의 성향이 강했다.
  이 모델은 논리적인 행동 시퀀스에 해당하는 아웃풋을 냈지만, 지정한 문법에서 자꾸 벗어나는 문제가 존재했다.
- GPT 3의 경우, Few Shot Learning으로 몇 개의 예시를 들어주면, 해당 예시의 말투, 문법, 스타일을 따라하려는 경향이 강했다.
  이 모델은 비교적 지정한 문법에서 벗어나지 않았으나, 논리성은 떨어지는 모습을 보였다.
- 우리는 안정적인 시연을 위해 안정성을 중요시 했고, finetuning endpoint가 열려있는 GPT 3를 사용하게 됐다.
---
![image](https://github.com/addinedu-amr-2th/robot-repo-4/assets/69943723/1207213e-2a10-4589-9e90-54b8722d4208)

- 이렇게 얻어진 행동 시퀀스는 Schedule Manager에 의해 ChatGPT 모델이 판단한 우선도에 따라 기존 스케쥴의 전방/후방에 배치되게 되고, Schedule Manager는 이를 하나씩 꺼내어 하드웨어에 명령을 내리고, 이 명령이 수행된 후 성공 여부를 나타내는 신호를 받아 이를 통해 하나의 동작의 완료를 인식하고 다음 동작을 순차적으로 실행하여 일련의 동작이 수행될 수 있게 하였다.
- 이론상 모든 복잡한 동작은 간단한 동작의 연속으로 구성되어 있으므로, 이는 ChatGPT가 사용자의 요구를 만족시키는 복잡한 동작들을 스스로 생성해냄을 의미한다.
---
![image](https://github.com/addinedu-amr-2th/robot-repo-4/assets/69943723/b79602f0-b263-41f6-a762-df188c803995)

- 위는 구체적인 동작의 예시이다.
- 사용자가 목이 마르다는 요청을 넣게 되면, ChatGPT는 이를 해소하기 위해 우선도를 높음으로 설정한 뒤, 
  병을 찾기, 병을 바라봄, 앞으로 감, 물병을 잡음, 사람을 찾음, 사람을 바라봄, 앞으로 감, 병을 내려놓음, 행복을 표현함의 일련의 과정을 출력으로 내놓는다.
- Schedule Manager는 이를 하나씩 다른 기기들에게 명령하게되고, 이로서 사용자의 요청에 대응하는 복잡한 동작이 이루어지게 된다.
---
그 결과 우리는 다음과 같이 사람과 친밀하게 상호작용하는 로봇을 만들 수 있었다.
![Animation](https://github.com/addinedu-amr-2th/robot-repo-4/assets/69943723/bf2a6e62-4831-43ac-bad3-9dc18b3c0750)
![Animation](https://github.com/addinedu-amr-2th/robot-repo-4/assets/69943723/c0a09812-fd23-4f6f-84f5-5903df42e91b)
![Animation](https://github.com/addinedu-amr-2th/robot-repo-4/assets/69943723/2968b2dd-470a-4d24-a221-27264b53c262)
## 의의
- 우리는 음성으로 소통하는 사람과 교감하는 로봇에 초점을 맞췄으나, 사용자의 요청에 맞는 일련의 실제동작을 만들어낸다는 점에서 교감 목적이 아닌, 음성 소통 방식이 아닌 로봇에도 위와 같은 방식의 프레임 워크를 사용할 수 있을 것이다.
- 이론상 ChatGPT3.5와 GPT3의 차이점은 양질의 대량 finetuning data이다. 
  즉, 양질의 데이터만 충분히 제공한다면 우리가 한 단순한 동작을 만드는 방법이 아닌 모터 등의 파라미터를 직접 조절하는 방식도 가능할 것으로 생각되며, 더 나아가서 그 자리에서 코드를 직접 작성하여 행동 노드조차 사람이 만들 필요가 없게 되는 시점까지도 갈 수 있을 지도 모른다.
- 인공지능을 적용하기 쉽지 않은 판단 영역에 대해서 거대 언어모델을 통해 유연한 행동제어가 가능할거라 기대하는게 가능할 것이다.
## 회고
- ChatGPT를 사용하는 논문들의 경우, 성능의 문제로 ChatGPT4를 주로 사용하는데, ChatGPT4의 경우 신청 후 허가받은 계정만이 사용 가능하기 때문에 기한 문제로 해당프로젝트에선 테스트해보지 못했다.
- 학습데이터로 자신이 누구고 어떤 상황인가에 대홰 로봇의 문법으로 대화하는 형식으로 구성된 기초지식 분야, 기초적인 행동 노드들을 하나씩 실행하는 기초동작 분야, 추상적인 요청에 복잡한 동작으로 대응해야 하는 복잡한 행동 분야의 세 분야의 데이터를 사용했는데, 각각의 데이터의 영향에 대해서는 아직 판단하지 못했다.
- Schedule Manager와 로봇 제어 노드를 분리했어야 했는데 그러지못했다. 때문에 코드 디버깅에 어려움이 있었다. 

## 코드 설명
#### stt_pkg : 음성 처리 및 ChatGPT와 관련된 패키지 
- audio_pub : 마이크 입력 음성 데이터를 스트리밍, 송신하는 노드
- audio_sub : 수신한 음성 데이터를 저장하여 재생하는 노드
- stt_sub : 수신한 음성 데이터를 저장하여 google STT API로 텍스트로 바꾸고 송신하는 노드
- tts_pub : 수신한 텍스트 데이터를 google TTS API로 음성으로 변경, 저장하고 재생하는 노드
- gpt_agent : 입력된 유저 request prompt로부터 이를 만족시키는 일련의 행동 시퀀스 텍스트를 ChatGPT API를 이용해 만들어내고 송신하는 노드
#### schedule_maker : 행동 시퀀스 스케쥴을 관리하고 로봇 구동부를 제어하는 패키지
- ch36_to_ch4, ch4_to_ch36 : ROS DOMAIN ID가 다른 두 기기간 정보를 중계하는 노드
- schedule_maker : gpt_agent 출력 시퀀스를 우선도에 따라 재배치하고 동작을 하나씩 꺼내서 실행시키는 노드
#### yolo_pkg : YOLO 모델의 정보를 schedule_maker 및 로봇팔 제어부에 전달해주는 패키지
- cam_pub : 
- cam_sub : 
- yolo_publisher : 
#### om_pkg : ChatGPT 명령에 의해 로봇팔을 제어하는 패키지
- om_teleopkey : 
## 발표 자료
- [발표 PPT](https://docs.google.com/presentation/d/1Db-Mb1rRizueh5NoOPT9ax4vFm7Z98R1q-yJYuG1zGs/edit?usp=sharing)
## 팀원 소개
팀 **ChatGPT와 함께 춤을~**
- [송승훈](https://github.com/addinedu-amr-2th/robot-repo-4/tree/ssh)
- [윤태웅](https://github.com/addinedu-amr-2th/robot-repo-4/tree/ytw)

