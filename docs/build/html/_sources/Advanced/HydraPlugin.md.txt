# 히드라 플러그인

히드라는 플러그인 구조를 가지고 있습니다. 플러그인 유형은 다음과 같습니다.

## 스위퍼(Sweeper)

스위퍼는 여러 작업안에서 명령어에 전달된 파라미터를 리스트로 변환합니다. 예를들어 기본 내장 스위퍼는 다음과 같습니다.

```
batch_size=128 optimizer=nesterov,adam learning_rate=0.01,0.1
```

그리고 다음과 같은 파라미터와 함께 4개의 작업을 생성합니다.

```
batch_size=128 optimizer=nesterov learning_rate=0.01
batch_size=128 optimizer=nesterov learning_rate=0.1
batch_size=128 optimizer=adam learning_rate=0.01
batch_size=128 optimizer=adam learning_rate=0.1
```

## 런처(Launcher)

런처들은 특정 환경에 대한 작업을 착수할 책임을 가지고 있ㅅ브니다. 런처는 위의 목록을 리스트 파라미터로 고지고 있는 배치이며, 각각의 작업을 시작한다. 작업은 구성하기 위해 설정 인자로 사용한다. 기본적인 런처는 로컬에서 작업을 간단하게 시작할 수 있다.

## 검색 경로 플러그인(SearchPathPlugin)

설정 경로 플러그인은 검색 경로를 조작할 수 있다. 이는 기본 Hidra 설정이 특정 환경에 더 적합하도록 영향을 미치거나, Hidra 앱에서 더 많은 설정을 사용할 수 있도록 검색 경로에 새 항목을 추가하는 데 사용할 수 있다.

SearchPathPlugin 플러그인은 히드라에 의해 자동으로 발견되며 설정이 설정되기 전에 검색 경로를 조작하기 위해 호출되고 있다.

또한 많은 다른 플러그인은 SearchPathPlugin을 구현하여 일단 설치된 설정 검색 경로에 설정을 추가한다.