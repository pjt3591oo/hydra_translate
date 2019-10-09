# 어플리케이션 패키징

당신은 설정과 함께 패키징할 수 있습니다. 이것은 예시는 [여기](https://github.com/facebookresearch/hydra/tree/master/examples/advanced/hydra_app_example)있습니다.

당신은 해당 코드와 함께 실행할 수 있습니다.

```bash
$ python examples/advanced/hydra_app_example/hydra_app/main.py
dataset:
  name: imagenet
  path: /datasets/imagenet
```

그것을 설치하려면 다음을 사용하세요

```bash
$ pip install examples/advanced/hydra_app_example
...
Successfully installed hydra-app-0.1
```

설치된 앱 사용하기

```bash
$ hydra_app
dataset:
  name: imagenet
  path: /datasets/imagenet
```

설치된 앱은 패키징된 설정파일을 사용합니다.