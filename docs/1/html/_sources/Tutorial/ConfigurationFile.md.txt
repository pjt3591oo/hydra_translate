# 설정파일

매번 커맨드 라인에서 명령어로 임의의 객체를 만드는 행위는 굉장히 귀찮습니다. 이것은 설정파일에서 생성할 수 있습니다.

설정파일: `config.yaml`

```yaml
db:
  driver: mysql
  user: omry
  pass: secret
```

데코레이터 `@hydra.main()` 의 `config_path`에 설정파일을 지정하여 전달할 수 있습니다.  `config_path`는 파이썬 파일의 상대경로 입니다.

파이썬 파일: `my_app.py`

```python
@hydra.main(config_path='config.yaml')
def my_app(cfg):
    print(cfg.pretty())
```

`config.yaml`은 너의 프로그램을 실행할 때 자동으로 읽습니다.

```bash
$ python my_app.py
db:
  driver: mysql
  pass: secret
  user: omry
```

당신은 커맨드라인에서 설정을 읽어올 때 값을 재정의 할 수 있습니다. 

```bash
$ python my_app.py db.user=root db.pass=1234
db:
  driver: mysql
  user: root
  pass: 1234
```



## 엄격모드(Strict mode)

`엄격모드`는 설정을 재정의 하거나 코드안에서  실수를 줄이기 위해 사용합니다. 엄격모드는 데코레이터 `@hydra.main`에서 `config_path` 인자를 위해 특정 파일경로를 전달할 때 기본적으로 활성화가 됩니다.

```python
@hydra.main(config_path='config.yaml')
def my_app(cfg):
    driver = cfg.db.driver # Okay
    user = cfg.db.user # Okay
    password = cfg.db.password # Not okay, there is no password field in db!
                               # This will result in a KeyError
```

또한 엄격모드는 커맨드라인에서 재정의할 때 실수를 잡아줍니다.

```bash
$ python my_app.py db.port=3306
Traceback (most recent call last):
...
KeyError: 'Accessing unknown key in a struct : db.port
```

엄격모드를 끄면 알 수 없는 키를 접근할 수 있으며 위의 재정의 예시와 아래의 코드 모두 실행합니다.

