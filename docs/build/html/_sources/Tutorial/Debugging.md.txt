# 디버깅

히드라는 향상된 디버깅을 위해 약간의 옵션을 제공합니다.



## 설정 출력

커맨드 라인에 `-c`또는 `--cfg`를 추가하여 작업을 실행하지 않고 작업에 사용도리 구성을 출력합니다.

```bash
# A normal run:
$ python tutorial/objects_example/my_app.py
MySQL connecting to localhost with user=root and password=1234
# just show the config without running your function:
$ python tutorial/objects_example/my_app.py -c
[2019-09-29 11:09:14,134] -
db:
  class: tutorial.objects_example.my_app.MySQLConnection
  params:
    host: localhost
    password: 1234
    user: root
```

출력된 설정은 커맨드라인에 전달된 인자와 함께 전달받은 설정입니다.

```bash
$ python tutorial/objects_example/my_app.py db=postgresql db.params.database=tutorial2 -c
[2019-09-29 11:14:55,977] -
db:
  class: tutorial.objects_example.my_app.PostgreSQLConnection
  params:
    database: tutorial2
    host: localhost
    password: 1234
    user: root
```

`--cfg` 플래그는 설정의 부분을 출력하기 위한 옵션 인자를 사용합니다.

* `job`: 당신의 설정
* `hydra`: 히드라의 설정
* `all`: 모든 설정, `job`과 `hydra`의 설정

경고: `-c`는 커맨드라인 마지막에 있어야 한다. `-c` 이후의 첫 번째 인자는 설정 유형으로 보여지며 해석합니다.



## 히드라 디버깅 모드

히드라는 `DEBUG` 수준에서 다음을 포함한 유용한 정보를 출력합니다. 

* installed plugins: 해당 환경에서 설치된 히드라 플러그인
* Config search path: 설정 검색 경로
* Composition trace: 설정하는데 사용된 설정파일, 순서 및 위치

이것은 종종 실행없이 설정을 보기위해 `-c`와 함께 사용된다. 다음은 출력 예제입니다.

```bash
$ python my_app.py hydra.verbose=hydra -c
[2019-09-29 13:35:46,780] - Installed Hydra Plugins
[2019-09-29 13:35:46,780] - ***********************
[2019-09-29 13:35:46,780] -     SearchPathPlugin:
[2019-09-29 13:35:46,780] -     -----------------
[2019-09-29 13:35:46,781] -     Sweeper:
[2019-09-29 13:35:46,781] -     --------
[2019-09-29 13:35:46,782] -             BasicSweeper
[2019-09-29 13:35:46,782] -     Launcher:
[2019-09-29 13:35:46,782] -     ---------
[2019-09-29 13:35:46,783] -             BasicLauncher
[2019-09-29 13:35:46,783] -
[2019-09-29 13:35:46,783] - Hydra config search path
[2019-09-29 13:35:46,783] - ************************
[2019-09-29 13:35:46,783] - | Provider | Search path                           |
[2019-09-29 13:35:46,783] - ----------------------------------------------------
[2019-09-29 13:35:46,783] - | hydra  | pkg://hydra.conf                        |
[2019-09-29 13:35:46,783] - | main   | /Users/omry/dev/hydra/tutorial/logging  |
[2019-09-29 13:35:46,783] -
[2019-09-29 13:35:46,783] - Composition trace
[2019-09-29 13:35:46,783] - *****************
[2019-09-29 13:35:46,783] - | Provider | Search path     | File      |
```

