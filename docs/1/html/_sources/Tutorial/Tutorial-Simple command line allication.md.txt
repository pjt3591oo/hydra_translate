# 간단한 명령 프로그램



이것은 설정을 출력하는 간단한 히드라 프로그램입니다. `my_app`는 코드를 입력하는 함수입니다. 우리는 히드라의 요소를 천천히 알아보며 해당 코드를 발전시킬 것 입니다.

[여기](https://github.com/facebookresearch/hydra/tree/master/examples)에 해당 튜토리얼 예시가 있습니다.

파이썬 파일: `my_app.py`

```python
import hydra
@hydra.main()
def my_app(cfg):
    print(cfg.pretty())
if __name__ == "__main__":
    my_app()
```

`cfg`는 너의 함수를 위해 설정된 객체인 [OmegaConf](https://omegaconf.readthedocs.io/en/latest/usage.html#access-and-manipulation)입니다. 너는 이 튜토리얼을 위해 OmagaConf의 깊은 이해를 하지않아도 된다.

우리는 임의로 계층적 객체를 생성하는 설정 인자를 전달할 수 있습니다.

```bash
$ python my_app.py db.driver=mysql db.user=omry db.pass=secret
db:
  driver: mysql
  pass: secret
  user: omry
```





