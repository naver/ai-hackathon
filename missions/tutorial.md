# 네이버 AI 해커톤 2018 NSML 튜토리얼

이 튜토리얼은 NSML을 설치하고 로그인하는 방법과 기본적인 명령어를 사용하는 방법, 모델을 구현할 때 참고할 사항을 간략하게 설명합니다.

NSML에 대한 더욱 자세한 설명은 다음 자료에서 확인할 수 있습니다.

* [deview 2017 영상](http://tv.naver.com/v/2301994)
* [NIPS 2017 영상](https://www.youtube.com/watch?v=3Qub0wL9Gwc&t=777s)
* [NSML: A Machine Learning Platform That Enables You to Focus on Your Models(논문)](https://arxiv.org/pdf/1712.05902.pdf)

## 시작하기

1. Quick-Guide : https://github.com/naver/ai-hackathon-2018/blob/master/quick-guide.md

### 설치

1. [Download NSML](https://hack.nsml.navercorp.com/download)에서 플랫폼에 맞는 클라이언트를 다운로드합니다.
2. 다운로드한 파일을 압축 해제합니다.

### 로그인

1. [NSML 웹](https://hack.nsml.navercorp.com)에서 Github 아이디를 이용해 로그인합니다.
2. 다음 명령어를 실행하여 NSML 클라이언트에 로그인합니다.

    ```bash
    $ PATH/TO/CLIENT/nsml login
    ```

    참고: 로그인은 160시간마다 만료됩니다. 160시간 후에 재시도할 때는 같은 과정으로 다시 로그인해주세요.

3. (선택) NSML 클라이언트를 로컬 머신 어디에서나 간단하게 이용할 수 있도록 `~/.bashrc` 혹은 `~/.bash_profile`에 아래와 같이 alias를 생성합니다.

    ```shell
    alias cmdyouwant = "/PATH/TO/CLIENT/nsml"
    # example
    alias nsml="/Downloads/nsml_client.macos.x86_64/nsml"
    ```

    이 튜토리얼에서는 alias를 `nsml`로 지정했다고 가정하고 튜토리얼을 진행하겠습니다.

이제 NSML을 이용하기 위한 준비가 끝났습니다.

## 명령어 사용하기

사용할 수 있는 명령어 목록을 보고 싶다면 다음 명령어를 실행하여 간단하게 확인할 수 있습니다.

```bash
$ nsml
```

각 명령어의 사용법과 옵션을 확인하고 싶다면 다음 명령어를 실행합니다. 이 튜토리얼에서는 기본적인 옵션만 설명합니다.

```bash
$ nsml COMMAND --help
# example
$ nsml login --help
```

이제 사용할 수 있는 명령어를 살펴보겠습니다.

### credit

먼저, 여러분에게 주어진 크레딧을 확인해보겠습니다.

```bash
$ nsml credit
# User : USER_NAME credit 000
```

크레딧은 [NSML 웹](https://hack.nsml.navercorp.com)에서 오른쪽 상단의 profile을 클릭하여 확인할 수도 있습니다.

크레딧은 하나의 GPU를 1분 사용하면 1만큼 차감됩니다. 남은 크레딧이 없으면 모델을 학습시키려 할 때 `NOT_ENOUGH_CREDIT`이라는 메시지가 표시됩니다. 해커톤을 진행하는 동안 자신에게 남은 크레딧을 수시로 확인하세요.

### dataset

해커톤을 위해 필요한 데이터는 모두 NSML 플랫폼에 있습니다. `dataset` 명령어를 실행하여 데이터셋 목록을 확인할 수 있습니다.

```bash
$ nsml dataset ls
```

다음과 같은 명령어를 실행하여 특정 문자열로 시작하는 데이터를 찾을 수도 있습니다.

```bash
$ nsml dataset search 'kin_*'
```

### run

홈페이지에는 해커톤의 두 문제의 예시가 업로드되어 있습니다. 그 중 지식iN 질문 유사도 모델을 구현한 `lstm.py` 파일을 이용해 설명드리겠습니다. 2개의 GPU를 사용하여 `kin_phase1` 데이터셋으로 이 모델을 학습시켜 보겠습니다.

```bash
# example
$ nsml run -d kin_phase1 -g 2 -e main.py
```

* `-d` 옵션은 사용할 데이터셋을 지정합니다.
* `-g` 옵션은 사용할 GPU의 개수를 지정합니다. NSML에서 실행하는 모든 모델은 기본적으로 GPU 한 개가 할당되므로, GPU를 한 개 사용하는 경우 따로 지정하지 않아도 됩니다.
* `-e` 옵션은 소스를 실행할 엔트리 포인트를 지정합니다. 만약 엔트리 포인트가 `main.py`라면 따로 지정하지 않아도 됩니다.

이 명령어를 실행하면 다음과 같이 세션이 하나 시작되었음을 알려주는 메시지가 표시됩니다.

```bash
...........
Session YOUR_ID/kin_phase1/SESSION_NUMBER started.
```

지금부터는 세션 정보인 `YOUR_ID/kin_phase1/SESSION_NUMBER`를 간단하게 `SESSION_NAME`(세션명)이라고 하겠습니다.

### 참고: GPU 사용 개수 지정

`-g` 옵션을 사용하여 GPU 사용 개수를 지정하는 경우, 모델에서 다음과 같이 `from nsml import GPU_NUM`으로 GPU 사용 개수(`GPU_NUM`)를 호출하여 이 값으로 cuda 사용 여부를 판단해야 합니다.

```python
# example
from nsml import GPU_NUM
.
.
.
if __name__ == "__main__":
    if GPU_NUM:
        torch.cuda.manual_seed(1)
```

### 참고: argument 전달

작성한 모델에 별도의 argument가 필요하다면 `run`을 실행할 때 다음과 같이 `-a "--argname value"` 옵션을 사용하여 argument를 전달할 수 있습니다.

```bash
$ nsml run -d kin_phase1 -g 2 -e main.py -a "--epochs 1000"
```

### logs

앞에서 실행한 세션의 학습 진행 상황을 살펴보겠습니다. `logs` 명령어를 실행하면 현재까지의 학습 진행 상황을 보여주는 로그가 출력됩니다.

```bash
$ nsml logs SESSION_NAME
```

업데이트되는 로그를 계속 보려면 NSML 웹에서 확인합니다.

웹 화면의 왼쪽에는 현재 NSML 플랫폼에 업로드되어 있는 데이터셋의 목록이 보입니다. 여기서 방금 사용한 데이터셋의 `>`를 클릭하면 실행한 세션 목록이 나타납니다. 세션 목록에서 로그를 보려는 세션명을 클릭하면 오른쪽에 해당 세션의 로그가 나타납니다.

`logs` 명령어는 이미 종료된 세션에도 실행할 수 있습니다. 웹에서는 상태가 **exited**인 세션을 클릭하여 확인할 수 있습니다.

작성한 모델에 `print()` 문이 있다면 해당 구문에 의해 출력된 로그도 확인할 수 있습니다.

### ps

여러 개의 세션을 실행시켰을 때 현재 진행 중인 세션 목록을 보려면 `ps` 명령어를 실행합니다.

```bash
$ nsml ps
```

어떤 argument를 전달하여 실행했는지, 어떤 상태인지 등 세션의 기본적인 정보를 확인할 수 있습니다.

#### 참고: ps 명령어의 옵션
`ps`명령어는 현재 **실행 중인** 세션만을 보여줍니다. 만약 종료된 세션까지 함께 보려면 `-a`를 입력하여 현재까지 생성한 모든 세션을 확인할 수 있습니다.

특정 데이터셋에 관련된 세션만을 확인하고 싶다면 `-d DATASET`을 추가하여 데이터셋에 대해 필터링된 세션을 확인할 수 있습니다.

만약 세션의 수가 너무 많아서 보기가 어렵다면 `-n NUMBER` 옵션을 사용해 최신 몇 개의 세션만을 확인할 수 있습니다.

### submit

이렇게 학습시킨 모델을 평가하고 리더보드에 업로드하는 방법을 알아보겠습니다.

현재 NSML 플랫폼에는 모델의 성능을 평가할 수 있는 테스트 데이터도 업로드되어 있습니다. 테스트 데이터는 학습 데이터와 쌍으로 존재하기 때문에 따로 데이터셋을 지정하지 않아도 됩니다.

`submit` 명령어는 특정 세션의 특정 이터레이션에 해당하는 모델을 이용해 성능을 평가하고 리더보드에 업로드합니다. 다음의 명령어를 실행하여 간단하게 모델을 평가해보세요.

```bash
$ nsml submit SESSION_NAME ITERATION
```

이제 [NSML 웹](https://hack.nsml.navercorp.com)에서 왼쪽의 데이터셋 목록에서 `kin_phase1`를 클릭하면 오른쪽에 리더보드가 나타납니다. 리더보드에서 순위를 확인하세요. 리더보드에서는 자신이 실행한 평가 세션 중 가장 점수가 높은 세션에 대한 정보만 보입니다.

### stop

백그라운드에서 실행되고 있는 세션을 중지하고 싶다면 `stop` 명령어를 실행합니다.

```bash
$ nsml stop SESSION_NAME
```

### resume

멈췄던 세션을 다시 시작하고 싶다면 `resume` 명령어를 실행합니다.

```bash
$ nsml resume SESSION_NAME
```

### fork

`fork` 명령어를 실행하여 현재 진행 중인 세션을 다른 파라미터로 복제(fork)할 수 있습니다.

```bash
$ nsml fork SESSION_NAME -a "--lr 0.02"
```

`fork` 명령어를 실행하여 복제한 후 다시 `ps` 명령어를 실행하면, 방금 시작된 세션이 어느 세션으로부터 복제한 세션인지에 대한 정보를 확인할 수 있습니다.

### 참고: submit, fork, resume의 argument 설정

다시 지식iN 분류 모델을 살펴보겠습니다. `submit`, `fork`, `resume` 명령어를 실행하려면 다음과 같은 코드 블럭을 모델에 작성해야 합니다.

```python
...
if __name__ == 'main__':
    #1
    args.add_argument('--pause', type=int, default=0)
    ...
    #2
    if config.pause:
        nsml.paused(scope=locals())
    ...
```

코드 블럭 #1은 NSML에 내부에서 학습된(혹은 학습이 진행 중인) 세션을 불러오기 위해 필요한 부분입니다. 실행할 때 argument를 입력받을 수 있도록 작성한 코드이지만 실제로 `submit`, `fork`, `resume` 명령어를 실행할 때는 따로 `--pause`를 argument로 전달하지 않습니다. 대신 코드 블럭 #2를 모델에 작성함으로써 `submit`, `fork`, `resume` 명령어를 실행할 때 NSML이 모델을 불러옵니다.

`submit` 명령어를 실행하려면 아래와 같이 한 가지 구현이 더 필요합니다.

```python
...
if __name__ == 'main__':
    args.add_argument('--mode', type=str, default='train')
...
```

위와 같이 `--mode`를 argument로 지정하면 NSML에서 `submit`을 실행할 때 해당 값을 이용합니다. 즉, `--pause`와 마찬가지로, 실제 실행할 때 따로 argument를 전달하지 않습니다.

`fork`와 `resume`은 이미 존재하는 세션을 토대로 새로운 세션을 만드는 명령어입니다. 따라서 기존의 모델 중 어느 부분으로부터 새로운 세션을 만들지에 대한 정보가 필요합니다. 이를 위해서는 아래와 같은 새로운 argument가 필요합니다.

```python
...
if __name__ == 'main__':

    args.add_argument('--iteration', type=str, default='0')
    ...

    if config.mode == 'train':
        full_iter = FULL_ITER
        for it in range(config.iteration, full_iter):
            TRAIN_YOUR_MODEL
...
```

위와 같이 코드를 작성하면 NSML은 실행할 때 주어지는 이터레이션 정보로부터 새로운 세션을 만들어 학습합니다.

### rm

여러분이 NSML에서 학습시킨 모델은 삭제되지 않습니다. 종료된 세션까지 확인하고 싶다면 `ps -a` 옵션으로 확인할 수 있습니다.

```bash
$ nsml ps -a
```

이 중 필요없는 세션이 있다면 `rm` 명령어로 삭제할 수 있습니다.

```bash
$ nsml rm SESSION_NAME
```

`rm` 명령어를 실행할 때 `SESSION_NAME`에 `*`와 `?` 와일드카드를 사용할 수 있습니다.

```bash
# example
$ nsml rm myId/kin_phase1/1*
$ nsml rm myId/kin_phase1/1?
```

## NSML 웹 사용하기

[NSML 웹](https://hack.nsml.navercorp.com)에서는 NSML 클라이언트에서는 수행할 수 없는 특별한 기능을 제공합니다. 바로 그래프와 디버깅입니다.

### 그래프

NSML 웹에서는 여러분이 학습시킨 모델에 대한 수치를 TensorBoard에서 확인할 수 있습니다. 이를 위해서는 코드에서 `nsml.report()` 함수를 사용해야 합니다. 지금까지와 마찬가지로 지식iN 분류 모델을 예로 확인해 보겠습니다.

```python
...

    for epoch in range(config.epochs):

        ...

        nsml.report(summary=True, scope=locals(), epoch=epoch, total_epoch=config.epochs, val_acc=val_acc,
                        train_acc=train_acc, train__loss=train_loss, val__loss=val_loss, min_val_loss=min_val_loss,
                        step=epoch)

...
```

위 코드는 매 epoch를 기준(`step=epoch`)으로 그래프를 그리도록 구현한 코드입니다. NSML에서 TensorBoard를 이용하여 그래프를 그리고 싶다면 report 함수의 필수 파라미터로 `step=` 값을 입력하고 그 외에 필요한 값을 입력하면 됩니다.

이제 모델을 학습시키고 웹에 접속해서 해당 세션을 찾아 클릭해 보세요. 페이지의 오른쪽 화면에서 세션 로그를 확인할 수 있습니다. 여기서 오른쪽 상단에 있는 **Graph** 버튼을 클릭하면 모델에 대한 정보가 그려진 TensorBoard 화면이 나타납니다.

### 디버깅

NSML 웹에서는 여러분이 학습시키고 있는 모델을 디버깅할 수 있습니다. 예시로 지식iN 분류 모델의 코드 일부를 보겠습니다.

```python
if __name__ == 'main__':

    ...
    config = args.parse_args()

    ...
```

이 코드에서 `config` 값을 확인하고 싶다면 해당 세션의 로그 화면에서 오른쪽 아래의 **Terminal** 버튼을 클릭한 다음, 입력 창이 나타나면 `config`를 입력합니다.

```bash
>>> config
```

### 모델 다운로드

모델을 로컬에서 확인하고 싶다면 웹에서 다운로드할 수 있습니다.

## 모델 구현하기

이제 모델 구현부를 살펴보겠습니다.

### 설정

여러분이 이용할 딥러닝 라이브러리와 파이썬 패키지는 `setup.py` 파일에 지정할 수 있습니다.

다음은 지식iN 분류 모델의 `setup.py` 파일입니다.

```python
#nsml: floydhub/pytorch:0.3.0-gpu.cuda8cudnn6-py3.17

from distutils.core import setup
setup(
    name='nsml Kin query',
    version='1.0',
    description='nsml',
    install_requires =[
    ]
)
```

첫 번째 줄에서 `#nsml:`로 지정한 것은 [Docker Hub](https://hub.docker.com)에서 찾은 딥러닝 라이브러리 이미지입니다. 이 값을 지정하지 않으면 PyTorch와 TensorFlow가 포함된 [NSML 기본 이미지](https://hub.docker.com/r/nsml/default_ml/)를 사용합니다.
지정하고 싶은 이미지가 있다면 Docker Hub에서 찾아 추가하면 됩니다.

### 모델

지식iN 분류 모델에는 다음과 같이 `nsml.bind()`에 들어가는 함수가 구현되어 있습니다. 만약 `save`, `load`, `infer`와 관련해서 특별히 구현하고 싶은 부분이 있다면 이 부분에 구현한 후 `nsml.save()`, `nsml.load()`, `nsml.infer()`로 호출해서 사용하면 됩니다.

```python
def bind_model(model, **kwargs):
    def save(filename, *args):
        checkpoint = {
            'model': model.state_dict(),
            'optimizer': kwargs['optimizer'].state_dict()
        }
        torch.save(checkpoint, filename)

    def load(filename, *args):
        checkpoint = torch.load(filename)
        model.load_state_dict(checkpoint['model'])
        if 'optimizer' in kwargs:
            kwargs['optimizer'].load_state_dict(checkpoint['optimizer'])
        print('Model loaded')

    def infer(input):
        print("input:", input)
        model.eval()
        tensor_seqs, tensor_lengths = load_batch_input_to_memory(input, has_targets=False)
        var_seqs = autograd.Variable(tensor_seqs).cuda()
        var_lengths = autograd.Variable(tensor_lengths).cuda()
        output_prediction = model(var_seqs, var_lengths)
        print("prediction:", output_prediction.data.squeeze(dim=1).tolist())
        return output_prediction.data.squeeze(dim=1).tolist()

    nsml.bind(save, load, infer)
.
.
.
if __name__ == "__main__":
    ...
    model = YOUR_MODEL()
    bind_model(model)
    # train
    .
```

NSML은 사용하는 라이브러리에 따라 동작하는 `save`와 `load`를 지원하지만 `infer` 함수는 직접 작성해야 합니다. `infer` 함수는 `nsml infer`와 `nsml submit`에서 사용되며, 기본 구조는 입력을 받아서 모델의 예측 결과를 반환합니다. 여기서 입력 및 결과물의 형식은 데이터 업로더가 직접 지정한 대로 따라서 만들면, `nsml submit`으로 모델의 성능을 평가하거나 직접 `nsml infer`로 여러 데이터에 대해서 실험해 볼 수 있습니다.

## NSML의 세계로

이제 NSML을 이용하기 위한 모든 준비가 끝났습니다. 제공된 baseline 모델을 참고하여 모델을 직접 구현해 보세요.
