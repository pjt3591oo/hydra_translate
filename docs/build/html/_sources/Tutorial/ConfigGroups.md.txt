# 설정 그룹(Config groups)

히드라 컨셉에서 가장 중요합니다.

당신은 PostgreSQL과 MySQL의 밴치마킹을 원한다고 가정합니다. 어플리케이션을 실행할 때, 당신은 MySQL 또는 PostgreSQL을 필요합니다. 그러나 둘 다 필요하지는 않습니다.

이 방법을 위해 히드라는 **설정그룹**를 이용합니다. 설정그룹은 상호배타적인 구성 파일 세트입니다.

설정그룹을 생성하기 위해 각각의 디비의 설정정보를 가진 디렉터리 `db` 를 생성합니다. 우리는 다수의 설정그룹을 가지고 있을것으로 예상되므로, 모든 설정파일을 `conf`으로 옮깁니다.

파이썬 파일: `my_app.py`

```python
@hydra.main(config_path="conf")
def my_app(cfg):
    print(cfg.pretty())
```

`config_path`는 [Tutorial-Simple command line example](https://cli.dev/docs/tutorial/simple_cli/)과 같이 설정 파일을 지정할 수 있습니다. 만약 `config_path`가 지정된 경우 그것은 최상위 디렉터리가 됩니다.

우리의 프로그램 디렉터리 구조는 보이는바와 같습니다.

```bash
├── conf
│   └── db
│       ├── mysql.yaml
│       └── postgresql.yaml
└── my_app.py
```

만약 당신이 이것을 실행한다면, 그것은 설정파일을 지정하지 않아 빈 설정값을 출력합니다.

```bash
$ python my_app.py
{}
```

당신은 명령어로부터 설정된 데이터베이스를 선택할 수 있습니다.

```bash
$ python my_app.py db=postgresql
db:
  driver: postgresql
  pass: drowssap
  timeout: 10
  user: postgre_user
```

전처럼, 당신은 설정파일을 재정의 할 수 있습니다.

```bash
$ python my_app.py db=postgresql db.timeout=20
db:
  driver: postgresql
  pass: drowssap
  timeout: 20
  user: postgre_user
```

이 것은 히드라의 강력한 간단한 예시입니다. 당신은 다수의 설정파일로 부터 컴포즈할 수 있습니다.