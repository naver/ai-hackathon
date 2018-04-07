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
