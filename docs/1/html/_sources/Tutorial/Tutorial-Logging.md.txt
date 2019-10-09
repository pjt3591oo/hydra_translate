# 로깅

사람들은 종종 설정하기 귀찮아 로깅을 사용하지 않습니다. 히드라는 당신을 위해 파이썬 로깅문제를 풀어줍니다.

기본적으로 히드라는 `info` 수준의 콘솔과 파일모두 기록합니다.

히드라의 로깅 예제입니다.

```python
import logging
# A logger for this file
log = logging.getLogger(__name__)
@hydra.main()
def my_app(_cfg):
    log.info("Info level message")
    log.debug("Debug level message")
$ python my_app.py
[2019-06-27 00:52:46,653][__main__][INFO] - Info level message
```

당신은 명령어 실행시 `hydra.vervose`를 재정의하여 `DEBUG` 수준으로 활성화할 수 있습니다. 

`hydra.verbose`는 부울, 문자열 또는 리스트일 수 있습니다.

* `hydra.verbose=true`: 모든 로깅레벨을 `DEBUG`로 설정
* `hydra.verbose=__main__`: `__main__` 로거의 로깅 레벨을 `DEBUG`로 설정
* `hydra.verbose[__main__,hydra]`: `__main__`와 히드라 로깅레벨을 `DEBUG`로 설정

출력예시는 다음과 같습니다.

```bash
$ python my_app.py hydra.verbose=[__main__,hydra]
[2019-09-29 13:06:00,880] - Installed Hydra Plugins
[2019-09-29 13:06:00,880] - ***********************
...
[2019-09-29 13:06:00,896][__main__][INFO] - Info level message
[2019-09-29 13:06:00,896][__main__][DEBUG] - Debug level message
```

로깅은 [커스텀마이징](https://cli.dev/docs/configure_hydra/logging/)할 수 있습니다.