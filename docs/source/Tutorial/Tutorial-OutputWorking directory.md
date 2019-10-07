# 출력 / 작업 디렉터리

히드라는 각 실행에 대한 디렉토리를 생성하고 해당 디렉토리 내에서 코드를 실행하여 각 실행에 대해 새 출력 디렉토리를 지정해야하는 문제를 해결합니다.

작업 디렉터리는 다음을 수행하는데 사용합니다.

* 응용프로그램 출력물 저장(예. 데이터베이스 덤프파일)
* 실행을 위한 히드라 출력 저장(설정구성, 로그 등)

당신이 매번 앱을 실행할 때, 새로운 디렉터리를 자동으로 생성합니다.

파이썬 파일: `my_app.py`

```python
import os
@hydra.main()
def my_app(_cfg):
    print("Working directory : {}".format(os.getcwd()))
$ python my_app.py
Working directory : /home/omry/dev/hydra/outputs/2019-09-25/15-16-17
$ python my_app.py
Working directory : /home/omry/dev/hydra/outputs/2019-09-25/15-16-19
```

해당 작업디렉터리중 하나를 살펴보겠습니다.

```bash
$ tree outputs/2019-09-25/15-16-17
outputs/2019-09-25/15-16-17
├── .hydra
│   ├── config.yaml
│   ├── hydra.yaml
│   └── overrides.yaml
└── my_app.log
```

우리는 히드라 출력 디렉터리(기본값에 의한 `.hydra`)와 어플리케이션 로그 파일을 가지고 있습니다. 구성 출력 디렉터리는 다음과 같습니다.

* `config.yaml`:  사용자 지정 구성의 덤프
* `hydra.yaml`: 히드라 구성의 덤프
* `ovverides.yaml`: 오버라이드에 사용된 명령어

그리고 메인 출력 디렉터리

* `my_app.log`: 이 실행을 위해 생성된 로그파일

워킹 디렉터리는 [커스텀마이징](https://cli.dev/docs/configure_hydra/workdir/) 할 수 있습니다.

