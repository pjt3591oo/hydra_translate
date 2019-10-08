# 로깅 커스텀마이징

히드라는 dictConfig 함수와 함께 파이썬 표준 로깅 라이프러리를 설정한다. 당신은 [여기서](https://docs.python.org/3/howto/logging.html) 더 많은것을 배울 수 있습니다. 두 가지 로깅 설정을 가지고 있으며, 한 가지는 히드라를 위한 것이며, 다른 한 가지는 실행중인 작업을 위한것 입니다.

이 예제는 다음과 같이 기본 로깅 동작을 변경하여 히드라 앱의 로깅동작시 사용자 지정 방식입니다.

* stdout에만 출력(파일생성 하지 않음)
* 보다 간단한 로그 라인 패턴 출력

프로젝트 구성:

```bash
$ tree
├── conf
│   ├── config.yaml
│   └── hydra
│       └── job_logging
│           └── custom.yaml
└── main.py
```

config.yaml은 기본적으로 애플리케이션을 사용자 정의 로깅으로 설정합니다.

설정파일: `config.yaml`

```yaml
defaults:
  - hydra/job_logging : custom
```

설정파일: `hydra/job_logging/logging.yaml`

```yaml
hydra:
  job_logging:
    formatters:
      simple:
        format: '[%(levelname)s] - %(message)s'
    root:
      handlers: [console]
```

기본 로깅은 다음과 같습니다.

```bash
$ python main.py hydra/job_logging=default
[2019-09-26 18:58:05,477][__main__][INFO] - Info level message
```

그리고 이것은 사용자 지정 로깅입니다.

```
$ python main.py
[INFO] - Info level message
```