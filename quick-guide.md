

Quick guide.. 터미널만 보고 따라하기. (command line을 복사/붙여넣기로 submit까지 5분).

```

# 해커톤 사이트에서 git clone하기.
ykkim@AL01205606~/demo $ git clone https://github.com/naver/ai-hackathon-2018.git
Cloning into 'ai-hackathon-2018'...
remote: Counting objects: 66, done.
remote: Compressing objects: 100% (51/51), done.
remote: Total 66 (delta 14), reused 58 (delta 11), pack-reused 0
Unpacking objects: 100% (66/66), done.

# git clone 잘되었나?
ykkim@AL01205606~/demo $ ls
ai-hackathon-2018

# nsml local 설치:
ykkim@AL01205606~/demo $ pip install git+https://github.com/n-CLAIR/nsml-local.git
Collecting git+https://github.com/n-CLAIR/nsml-local.git
  Cloning https://github.com/n-CLAIR/nsml-local.git to /private/var/folders/j0/gyhk9qgd69l9g21bf8b1q5200000gp/T/pip-d5rtkzo6-build
  Requirement already satisfied (use --upgrade to upgrade): nsml==0.0 from git+https://github.com/n-CLAIR/nsml-local.git in /Users/ykkim/anaconda3/lib/python3.6/site-packages/nsml-0.0-py3.6.egg

# nsml CLI 다운로드 (mac에서..)
ykkim@AL01205606~/demo $ curl -L -o cli.tar.gz https://github.com/n-CLAIR/File-download/raw/master/nsml/hack/nsml_client.darwin.amd64.hack.tar.gz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   175  100   175    0     0     87      0  0:00:02  0:00:02 --:--:--    83
100 9439k  100 9439k    0     0  2359k      0  0:00:04  0:00:04 --:--:-- 5630k

# tar.gz 압축 풀기.
ykkim@AL01205606~/demo $ tar xvpf cli.tar.gz 
x nsml_client.darwin.amd64.hack/
x nsml_client.darwin.amd64.hack/nsml
x nsml_client.darwin.amd64.hack/README.hack.txt

# nsml이 있는 위치로 이동
ykkim@AL01205606~/demo $ cd nsml_client.darwin.amd64.hack/

# nsml을 경로에 추가
ykkim@AL01205606~/demo/nsml_client.darwin.amd64.hack $ export PATH=$PATH:`pwd` 

# nsml이 동작하는지 확인.
ykkim@AL01205606~/demo/nsml_client.darwin.amd64.hack $ nsml --version
Built: 2018-03-22T00:48:15+09:00
Go: go1.10
GOARCH: amd64
GOOS: darwin
Protocol: 20180321
Revision: c41e9558d212fd200f25ead58863a9ebcb6477e7 (not-modified)
Preconfigured connection: hack.cli.nsml.navercorp.com


# kin example code가 있는 디렉토리로 이동
ykkim@AL01205606~/demo $ cd ai-hackathon-2018/missions/examples/kin/example/

# dataset 이름 확인
ykkim@AL01205606~/demo/ai-hackathon-2018/missions/examples/kin/example $ nsml dataset ls
Name          Size      Last Modified    Author      Description       Tag
------------  --------  ---------------  ----------  ----------------  -----
kin_phase1    3.5 MB    4 hours ago      nsml-admin  만료 날짜: 4/9 11:00
movie_phase1  24.06 MB  4 hours ago      nsml-admin  만료 날짜: 4/9 11:00

# kin_phase1과 함께 main.py을 nsml로 보내어 GPU에서 실행
ykkim@AL01205606~/demo/ai-hackathon-2018/missions/examples/kin/example $ nsml run -d kin_phase1
...........
Session nsml-admin/kin_phase1/3 started.

# 잘 돌고 있는지 process 상태 확인. (-a: 완료된 것도 포함)
ykkim@AL01205606~/demo/ai-hackathon-2018/missions/examples/kin/example $ nsml ps -a
Name                       Created         Args     Status    Summary                                     Description
-------------------------  --------------  -------  --------  ------------------------------------------  -------------
nsml-admin/kin_phase1/3    just now        main.py  Running   epoch=1, epoch_total=10, train/loss=0.688
nsml-admin/movie_phase1/2  4 minutes ago   main.py  Running   epoch=2, epoch_total=10, train/loss=12.804
nsml-admin/kin_phase1/2    14 minutes ago  main.py  Exited    epoch=9, epoch_total=10, train/loss=0.621
nsml-admin/movie_phase1/1  4 hours ago     main.py  Exited    epoch=9, epoch_total=10, train/loss=12.808
nsml-admin/kin_phase1/1    4 hours ago     main.py  Exited    epoch=9, epoch_total=10, train/loss=0.623

# 실행이 완료되었으면..  nsml model ls 로 적절한 Checkpoint 확인.(학습 중간중간 저장된 model)
ykkim@AL01205606~/demo/ai-hackathon-2018/missions/examples/kin/example $ nsml model ls nsml-admin/kin_phase1/3 
Checkpoint    Last Modified    Elapsed    Summary
------------  ---------------  ---------  --------------------------------------------------------------
0             seconds ago      0.296      train/loss=0.7664164423942565, epoch=0, step=0, epoch_total=10
1             seconds ago      1.413      train/loss=0.688490229845047, epoch=1, step=1, epoch_total=10
2             seconds ago      1.434      train/loss=0.6506421864032745, epoch=2, step=2, epoch_total=10
3             seconds ago      1.425      train/loss=0.6539466977119446, epoch=3, step=3, epoch_total=10
4             seconds ago      1.422      train/loss=0.647806578874588, epoch=4, step=4, epoch_total=10
5             seconds ago      1.453      train/loss=0.6433648660778999, epoch=5, step=5, epoch_total=10
6             seconds ago      1.451      train/loss=0.6380701988935471, epoch=6, step=6, epoch_total=10
7             seconds ago      1.471      train/loss=0.6320644378662109, epoch=7, step=7, epoch_total=10
8             seconds ago      1.453      train/loss=0.6266655698418617, epoch=8, step=8, epoch_total=10
9             seconds ago      1.463      train/loss=0.6218963131308556, epoch=9, step=9, epoch_total=10

# model선택하였으면, submit 명령으로 leaderboard에 제출.
ykkim@AL01205606~/demo/ai-hackathon-2018/missions/examples/kin/example $ nsml submit nsml-admin/kin_phase1/3 9
....................ykkim@AL01205606~/demo/ai-hackathon-2018/missions/examples/kin/example $ 
```

약간 더 자세히..

https://docs.google.com/presentation/d/1gpCpmVu4lks2kvH_kMJkSRqsr6ZQT8Y1nSac40MzVzI/edit#slide=id.g3638547719_0_0
