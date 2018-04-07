# FAQ For NSML

#### 0. 빠르게 submit까지 해보고 싶은데요. 핵심만 알려면 어떻게 하나요?
아래 링크를 따라하시면 빠르게 submit해보실 수 있습니다. 
https://github.com/naver/ai-hackathon-2018/blob/master/quick-guide.md

#### 1. nsml local에서 IS_DATASET import 오류가 아래와 같이 나와요.
```
Traceback (most recent call last):

  File "main.py", line 12, in <module>

    from nsml import DATASET_PATH, IS_DATASET, GPU_NUM

ImportError: cannot import name 'IS_DATASET'
```
답변)
샘플코드는 과거 버전입니다.  배포된 코드는 IS_DATASET -> HAS_DATASET으로 변경되어 배포되었습니다. 
문제 해결을 위해서 아래 링크에서 example을 새로 받으세요.
https://github.com/naver/ai-hackathon-2018/tree/master/missions/examples
​
#### 2. nsml-local를 설치 하여, local에서 작업을 하려고 하는데 아래와 같이 nsml을 인식하지 못합니다.
 (간단한 package link 문제인거 같은데...)
```
Traceback (most recent call last):
  File "main.py", line 30, in <module>
    import nsml
ModuleNotFoundError: No module named 'nsml'
```
답변) nsml local의 재설치를 위해서 아래 명령어를 실행하시면 됩니다. uninstall 및 install을 수행합니다. 
pip uninstall git+https://github.com/n-CLAIR/nsml-local.git;pip install git+https://github.com/n-CLAIR/nsml-local.git

#### 3. 영화 평점 리뷰에 대한 Tensorflow 예제는 없나요?
답변) 네에, 영화 평점 리뷰는 PyTorch로 되어있습니다. 하지만, Kin예제는 Tensorflow로 되어 있습니다. 두 예제가 비슷하므로 어렵지 않게 변경이 가능할 것으로 예상됩니다. 

#### 4. submit을 했는데, leaderboard에 변경이 없어요. 과거 submit history를 확인하는 방법이 있나요?
답변) leaderboard에 제출된 값중에서 가장 좋은 결과 1건만 보여주고 있습니다. 추가로 자신이 제출한 모든 이력을 확인하는 방법이 있습니다. 아래 그림과 같이 leaderboard에서 public 탭을 private으로 변경하시면 됩니다.

![leaderboard-private](./res/leaderboard.png)

#### 5. credit은 어떻게 지급되나나요?
답변) credit은 매일 10:30(am) ~ 11:00(am) 사이에 지급됩니다. CLI의 nsml credit 명령과 web화면에서 확인 가능합니다. 

#### 6. 팀원이 두명인데, 한명만 로그인이 되어요. 정상인가요?
답변) 네에, 현재 팀원1로 등록된 분만 nsml에 접근하시어 사용가능합니다. credit 또한 1명의 계정에만 지급됩니다. 

#### 7. nsml에서 외부 python package를 설치할 수 있나요?
답변) 네에, setup.py를 통하여 추가 설치를 할 수 있습니다. 

#### 8. nsml에서 pythone이외의 lib를 사용할 방법이 있나요?
답변) 네에, 몇가지 조건을 만족하면 외부 lib를 사용할 수 있습니다. 일단, docker image를 생성하여 setup.py의 맨 위줄 #nsml: ... 형식으로 docker image의 이름을 명시하여야 합니다.
관련 내용은 아래 링크에서 참고하실 수 있습니다.  https://github.com/naver/ai-hackathon-2018/blob/master/missions/tutorial.md#%EB%AA%A8%EB%8D%B8-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0 

#### 9. setup.py에는 항상 #nsml로 시작해야하나요?
답변) setup.py에 첫 줄에 있는 #nsml: ... 은 nsml에서 docker Hub에 있는 외부 Docker image를 사용할 때만 적으면 됩니다. 실제로, 샘플로 제공된 kin과 movie review 예제 중에서 한쪽만 #nsml: ... 형식을 취하고 있습니다. 이곳에 아무것도 적지 않으면, nsml은 nsml사용을 위해서 생성해둔 기본 docker image를 사용하게 됩니다. 


