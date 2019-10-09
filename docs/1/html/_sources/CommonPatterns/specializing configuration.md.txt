# 전문화된 설정

경우에 따라 원하는 설정이 다른 설정에 따라 달라집니다. 예를들어, 당신은 데이터베이스 cifar10를 선택한 경우 Alexnet 모델에서 5개의 레이어를 사용하고, 그렇지 않을경우 기본값 7개의 레이어를 사용합니다.

우리는 다음과 같은 코드로 시작할 수 있습니다.

## config.yaml 초기화

```yaml
defaults:
  - dataset: imagenet
  - model: alexnet
```

우리는 선택된 데이터 셋과 모델을 바탕으로 특별한 설정을 구성하기를 원합니다. 우리는 cifar10과 alexnet만 적용하고 나머지 3가지는 적용하지 않습니다.

OmegaConf는 `yaml`형태로 만들어주는 것을 지원하므로, 런타임에 다른 값의 함수가 될 값을 구성할 수 있습니다. 이 아이디어는 다른 두 값에 따라 파일 이름을 로드하는 다른 요소를 더할 수 있다. 

## config.yaml 수정

```yaml
defaults:
  - dataset: imagenet
  - model: alexnet
  - dataset_model: ${defaults.0.dataset}_${defaults.1.model}
    optional: true
```

해당 설정에 대한 분해

### dataset_model

`dataset_model`은 임의에 디렉터리에 있습니다. 그것은 `dataset/model`과 같이 디렉터리를 포함하여 의미가 있는 유니크할 수 있습니다.

### ${defaults.0.dataset}_${defaults.1.model}

`${defaults.0.dataset}_${defaults.1.model}` 값은 OmegaConf의 [변수](https://omegaconf.readthedocs.io/en/latest/usage.html#variable-interpolation)를 사용합니다. 실행시 defaults.dataset과 defalts.model에 따라 imagenet_alexnet 또는 cifar_resnet-depending으로 해석됩니다. 기본값에 목록을 포함하기 때문에 혼란스러울 수 있습니다. (향후에 이것을 개선하기를 희망합니다)

### optional: true

히드라는 기본값에 지정된 설정이 존재하지 않으면 오류와 함께 실패합니다. 우리는 cifar10 + alexnet 조합만 가지고 싶습니다. 이 것은 4가지 경우가 아닙니다. 표시옵션: `options:true`는 파일을 찾을 수 없으면 계속 진행하라고 히드라에게 알립니다.

전문화된 설정을 할 때, 전체가 아닌 다른것을 지정하려고 합니다. 우리는 cifar에 대해 훈련할 때 alexnet에 대한 모데이 5개의 레이어를 갖기를 원합니다.

## dataset_model/cifar10_alexnet.yaml

```yaml
model:
  num_layers: 5
```

검사해보자. 기본으로 실행하면 imagent를 사용하므로 다음과 같은 특수 버전을 얻지 못합니다.

```bash
$ python example.py 
dataset:
  name: imagenet
  path: /datasets/imagenet
model:
  num_layers: 7
  type: alexnet
```

cifar10 데이터셋과 함께 실행하면, 우리는 5개의 레이어를 얻을 수 있습니다.

```bash
$ python example.py dataset=cifar10
dataset:
  name: cifar10
  path: /datasets/cifar10
model:
  num_layers: 5
  type: alexnet
```