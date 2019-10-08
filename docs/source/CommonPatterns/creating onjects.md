# 객체생성

응용프로그램에서 다른 동작을 수행하는 가장 좋은방법은 인터페이스 구현을 인스턴스화하는 것 입니다.

한 데이터베이스 인터페이스는 다른 데이터베이스 드라이버에 의해 구현된 `connect()` 메서드를 가집니다.

```python
class DBConnection:
    def connect(self):
        pass
class MySQLConnection(DBConnection):
    def __init__(self, host, user, password):
        self.host = host
        self.user = user
        self.password = password
    def connect(self):
        print(
            "MySQL connecting to {} with user={} and password={}".format(
                self.host, self.user, self.password
            )
        )
class PostgreSQLConnection(DBConnection):
    def __init__(self, host, user, password, database):
        self.host = host
        self.user = user
        self.password = password
        self.database = database
    def connect(self):
        print(
            "PostgreSQL connecting to {} "
            "with user={} and password={} and database={}".format(
                self.host, self.user, self.password, self.database
            )
        )
```

이것을 제공하기 위해, 우리는 우리는 병렬적인 설정구조를 가질 수 있습니다.

```bash
conf/
├── config.yaml
└── db
    ├── mysql.yaml
    └── postgresql.yaml
```

설정파일: `config.yaml`

```yaml
defaults:
  - db: mysql
```

설정파일: `db.mysql.yaml`

```yaml
model:
db:
  class: tutorial.objects_example.objects.MySQLConnection
  params:
    host: localhost
    user: root
    password: 1234
```

설정파일: `db/postgresql.yaml`

```yaml
db:
  class: tutorial.objects_example.objects.PostgreSQLConnection
  params:
    host: localhost
    user: root
    password: 1234
    database: tutorial
```

이를통해, 당신은 한줄의 코드로 객체를 인스턴스화 할 수 있습니다.

```python
@hydra.main(config_path="conf/config.yaml")
def my_app(cfg):
    connection = hydra.utils.instantiate(cfg.db)
    connection.connect()
```

MySQL은 파일에 따라 `config.yaml`의 기본값 입니다.

```bash
$ python my_app.py
MySQL connecting to localhost with user=root and password=1234
```

인스턴스화 된 객체 클래스를 변경하고 커맨드라인에서 값을 재정의 하세요.

```base
$ python my_app.py db=postgresql db.params.password=abcde
PostgreSQL connecting to localhost with user=root and password=abcde and database=tutorial
```