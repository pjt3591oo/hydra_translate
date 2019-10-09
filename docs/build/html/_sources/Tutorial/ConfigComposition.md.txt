# 설정 컴포지션

관리자는 다음과 같은 생각을 가집니다:  그녀(관리자)는 my_app에서 제공하는 모든 데이터베이서의 스키마 생성을 제공하길 원합니다. 또한 두 가지의 UI(데이터베이스를 생성하고 볼 수 있는 전체 UI와 특정뷰 UI)를 가지고 있기를 원합니다.  당신은 이벤트를 시작하기 전, 그녀에게 제공하기 위해 필요한 데이터 베이스 스키마 3개를 이미 가지고 있고, 더 많은 것들이 곧 제공될 것이라고 말합니다. 당신은 이미 복잡함에 땀을 흘리고 있을것 입니다. 당신은 이것이 시작에 불과하며 그녀가 어떤 생각을 가질지 누가 알겠습니까??!!??!?!?!?!?!?!?

히드라를 이용하여 풀기위해, 우리는 더 많은 설정그룹이 필요합니다. 설정그룹에 `schema`와 `ui`를 추가합니다.

```bash
├── conf
│   ├── config.yaml
│   ├── db
│   │   ├── mysql.yaml
│   │   └── postgresql.yaml
│   ├── schema
│   │   ├── school.yaml
│   │   ├── support.yaml
│   │   └── warehouse.yaml
│   └── ui
│       ├── full.yaml
│       └── view.yaml
└── my_app.py
```

이것의 포인트는 2개의 데이터베이스를 지원하며, 3개의 스키마, 2개의 UI를 이미 가지고 있습니다. 이것은 12개의 경우를 가지고 있습니다. 지원되는 다른 데이터베이스를 추가하면 18개의 경우를 가집니다. 18개의 파일생성은 좋은 생각이 아닙니다. 만약 당신은 `db.user`를 `db.username`으로 바꾸기를 원한다면 18번의 시도!를 할 것입니다.

컴포지션은 이러한 상황을 도와줍니다.



설정파일: `config.yaml`

```yaml
defaults:
  - db: mysql
  - ui: full
  - schema: school
```

기본기능:

	* 만약 같은 값으로 정의된 n개의 설정을 가지고 있다면, n 번째 설정이 높은 우선순위를 가집니다.
	* 만약 n개의 설정이 동일한 사전에 기여하는 경우 결과는 결합된 사전이됩니다.

이것을 실행할 때 우리는 `mysql`, `full` UI와 `school` 스키마로 구성합니다.

```bash
$ python my_app.py
db:
  driver: mysql
  pass: secret
  user: omry
schema:
  database: school
  tables:
  - fields:
    - name: string
    - class: int
    name: students
  - fields:
    - profession: string
    - time: data
    - class: int
    name: exams
ui:
  windows:
    create_db: true
    view: true
```

비슷한 방식으로 `db=postgresql`을 전달하여 11개의 조합을 만들 수 있습니다.

