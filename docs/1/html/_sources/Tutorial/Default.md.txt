# 기본값

만약, `MySQL`사용을 결정하면 어플리케이션 실행마다 `db=mysql`를 입력하지 않을것입니다.

이때 설정파일에 `default`를 추가할 수 있습니다.



## 설정그룹 기본값

설정파일: `config.yaml`

```yaml
defaults:
    - db: mysql
```

당신은 `@hydra.main() `의 config_path`로 `config.yaml`를 설정한다는 점을 기억해야 합니다.

```python
@hydra.main(config_path='conf/config.yaml')
def my_app(cfg):
    print(cfg.pretty())
```

당신은 수정됭 프로그램을 실행할 때, 기본값에 의해 `MySQL`을 가져옵니다.

```bash
$ python my_app.py
db:
  driver: mysql
  pass: secret
  user: omry
```



## 설정그룹 기본값 재정의

당신은 여전히 PostreSQL을 가져올 수 있습니다. 그리고 개별적인 값을 재정의 할 수 있습니다.

```bash
$ python my_app.py db=postgresql db.timeout=20
db:
  driver: postgresql
  pass: drowssap
  timeout: 20
  user: postgre_user
```

당신은 커맨드라인에서 `null`이 할당되서 읽어오는 것을 막을 수 있습니다.

```bash
$ python my_app.py db=null
{}
```



## Non-config group defaults

종종 병합하려는 설정파일이 설정그룹에 속하지 않을 수 있습니다. 다음과 같이 설정 디렉터리로 부터 `some_file.yaml`을 읽어오십시오.

```
defaults:
    - some_file
```

`db`와 같은 `설정그룹`의 일부인 설정파일은 재정의할 수 없다. 