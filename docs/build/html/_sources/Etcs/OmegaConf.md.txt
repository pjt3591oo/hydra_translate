# OmegaConf

[공식문서](https://omegaconf.readthedocs.io/en/latest/usage.html#access-and-manipulation)

## 설치

pip 이용한 설치

```bash
pip install omegaconf
```

## 생성

### 빈값

```bash
>>> from omegaconf import OmegaConf
>>> conf = OmegaConf.create()
>>> print(conf.pretty())
{}
```

### 딕셔너리를 이용한 생성

```bash
>>> conf = OmegaConf.create(dict(k='v',list=[1,dict(a='1',b='2')]))
>>> print(conf.pretty())
k: v
list:
- 1
- a: '1'
  b: '2'
```

### 리스트를 이용한 생성

```bash
>>> conf = OmegaConf.create([1, dict(a=10, b=dict(a=10))])
>>> print(conf.pretty())
- 1
- a: 10
  b:
    a: 10
```

### yaml 파일을 이용한 생성

```bash
>>> conf = OmegaConf.load('source/example.yaml')
>>> # Output is identical to the yaml file
>>> print(conf.pretty())
log:
  file: ???
  rotation: 3600
server:
  port: 80
users:
- user1
- user2
```

### yaml 문자열을 이용한 생성

```bash
>>> conf = OmegaConf.create("a: b\nb: c\nlist:\n- item1\n- item2\n")
>>> print(conf.pretty())
a: b
b: c
list:
- item1
- item2
```

### 점으로 구분된 아이템으로 이루어진 리스트를 이용한 생성

```bash
>>> dot_list = ["a.aa.aaa=1", "a.aa.bbb=2", "a.bb.aaa=3", "a.bb.bbb=4"]
>>> conf = OmegaConf.from_dotlist(dot_list)
>>> print(conf.pretty())
a:
  aa:
    aaa: 1
    bbb: 2
  bb:
    aaa: 3
    bbb: 4
```

### 커맨드라인 인자를 이용한 생성

```bash
>>> # Simulating command line arguments
>>> sys.argv = ['your-program.py', 'server.port=82', 'log.file=log2.txt']
>>> conf = OmegaConf.from_cli()
>>> print(conf.pretty())
log:
  file: log2.txt
server:
  port: 82
```

## 접근, 조작

이 색션을 위해 사용하는 yaml 파일입니다.

```yaml
server:
  port: 80
log:
  file: ???
  rotation: 3600
users:
  - user1
  - user2
```

### 접근

```python
>>> # object style access of dictionary elements
>>> conf.server.port
80

>>> # dictionary style access
>>> conf['log']['rotation']
3600

>>> # items in list
>>> conf.users[0]
'user1'
```

### 기본값

당신은 코드에서 해당값에 직접적으로 접근하여 기본값을 제공할 수 있습니다.

```python
>>> # providing default values
>>> conf.missing_key or 'a default value'
'a default value'

>>> conf.get('missing_key', 'a default value')
'a default value'
```

### 의무적인 값

값 사용?? 접근하기 전에 설정해야 하는 매개변수를 지시합니다.

```bash
>>> conf.log.file
Traceback (most recent call last):
...
omegaconf.MissingMandatoryValue: log.file
```

### 조작

```python
>>> # Changing existing keys
>>> conf.server.port = 81

>>> # Adding new keys
>>> conf.server.hostname = "localhost"

>>> # Adding a new dictionary
>>> conf.database = {'hostname': 'database01', 'port': 3306}
```

## 변수 보간

보간된 변수는 설정안의 다른 지점에서 점을 이용하여 경로를 설정할 수 있고, 이 경우 이 값은 다른 노드의 값이 됩니다.

입력되는 yaml 파일:

```bash
server:
  host: localhost
  port: 80

client:
  url: http://${server.host}:${server.port}/
  server_port: ${server.port}
```

예제: 

```python
>>> conf = OmegaConf.load('source/config_interpolation.yaml')
>>> # Primitive interpolation types are inherited from the referenced value
>>> print(conf.client.server_port)
80
>>> print(type(conf.client.server_port).__name__)
int

>>> # Composite interpolation types are always string
>>> print(conf.client.url)
http://localhost:80/
>>> print(type(conf.client.url).__name__)
str
```

### 환경변수 보간

환경변수 보간도 제공합니다.

입력되는 yaml 파일:

```bash
user:
  name: ${env:USER}
  home: /home/${env:USER}
```

```python
>>> conf = OmegaConf.load('source/env_interpolation.yaml')
>>> print(conf.user.name)
omry
>>> print(conf.user.home)
/home/omry
```

### 사용자 정의 보간

당신은 사용자 정의 resolver를 사용하여 보간타입을 추가할 수 있습니다. 이 예제는 전달한 값에 10을 더하는 resolver를 생성합니다.

```python
>>> OmegaConf.register_resolver("plus_10", lambda x: int(x) + 10)
>>> c = OmegaConf.create({'key': '${plus_10:990}'})
>>> c.key
1000
```

## 설정 합치기

설정을 합치면 작업의 각 변동에 맞춰 논리 구성요소에 맞는 설정 파일을 재구성할 수 있습니다.

머신러닝 표현식 예제:

```python
conf = OmegaConf.merge(base_cfg, model_cfg, optimizer_cfg, dataset_cfg)
```

웹 서버 설정 예제:

```python
conf = OmegaConf.merge(server_cfg, plugin1_cfg, site1_cfg, site2_cfg)
```

다음 예제는 파일에서 두 개의 설정, cli에서 하나의 설정을 생성합니다.

example2.yaml 파일:

```yaml
server:
  port: 80
users:
  - user1
  - user2

```

example3.yaml 파일:

```yaml
log:
  file: log.txt
```

```python
>>> from omegaconf import OmegaConf
>>> import sys
>>> base_conf = OmegaConf.load('source/example2.yaml')
>>> second_conf = OmegaConf.load('source/example3.yaml')

>>> # Merge configs:
>>> conf = OmegaConf.merge(base_conf, second_conf)

>>> # Simulate command line arguments
>>> sys.argv = ['program.py', 'server.port=82']
>>> # Merge with cli arguments
>>> conf.merge_with_cli()
>>> print(conf.pretty())
log:
  file: log.txt
server:
  port: 82
users:
- user1
- user2
```

## 플래그 설정

> 참고: 플래그는 1.3.0 버전(이전버전)에 새로 추가된 요소입니다. API는 안정성을 고려하지 않고, 1.3.0이 출시되기 전에 변경될 수 있습니다.

OmegaConf는 몇개의 설정 플래그를 제공합니다. 플래그 설정은 모든 노드의 설정에서 플래그를 설정할 수 있습니다. 만약 플래그 설정이 안 됬을 경우, 부모노드로 부터 값을 상속받습니다. 루트노드에서 상속된 기본값은 항상 거짓입니다.

### 읽기 전용 플래그

읽기 전용 설정이 된 플래그는 수정할 수 없습니다. 수정을 시도할 경우 `omegaconf.ReadonlyConfigError ` 익셉션을 발생합니다.

```python
>>> conf = OmegaConf.create(dict(a=dict(b=10)))
>>> OmegaConf.set_readonly(conf, True)
>>> conf.a.b = 20
Traceback (most recent call last):
...
omegaconf.ReadonlyConfigError: a.b
```

당신은 설정객체로 부터 읽기 전용 플래그 기능을 지울 수 있습니다.

```python
>>> import omegaconf
>>> conf = OmegaConf.create(dict(a=dict(b=10)))
>>> OmegaConf.set_readonly(conf, True)
>>> with omegaconf.read_write(conf):
...   conf.a.b = 20
>>> conf.a.b
20
```

### 플래그 구조

기본값에 의해, OmegaConf 디셔너리는 정의되지 않은 필드를 읽고 쓰는것을 허용합니다. 만약 필드가 존재하지않는다면, None을 반환하거나 새로운 필드를 생성합니다. 이 행동을 변화하는 것은 가끔 도움됩니다.

```python
>>> conf = OmegaConf.create(dict(a=dict(aa=10, bb=20)))
>>> OmegaConf.set_struct(conf, True)
>>> conf.a.cc = 30
Traceback (most recent call last):
...
KeyError: 'Accessing unknown key in a struct : a.cc'
```

당신은 설정객체로 부터 플래그의 구조를 지울 수 있습니다.

```python
>>> import omegaconf
>>> conf = OmegaConf.create(dict(a=dict(aa=10, bb=20)))
>>> OmegaConf.set_struct(conf, True)
>>> with omegaconf.open_dict(conf):
...   conf.a.cc = 30
>>> conf.a.cc
30
```