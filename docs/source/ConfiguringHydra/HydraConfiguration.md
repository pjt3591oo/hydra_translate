# 히드라 설정

아래의 설정에서 히드라 분기에 속하는 것은 다양한 방법으로 재지정할 수 있습니다. 사용자 지정을 적용하는 방법에 대한 자세한 내용은 [소개](./introduction.md)를 참조하세요.

```yaml
hydra:
  run:
    # Output directory for normal runs
    dir: ./outputs/${now:%Y-%m-%d_%H-%M-%S}
  sweep:
    # Output directory for sweep runs
    dir: /checkpoint/${env:USER}/outputs/${now:%Y-%m-%d_%H-%M-%S}
    # Output sub directory for sweep runs.
    subdir: ${hydra.job.num}_${hydra.job.id}
```

# 실행중 가변인자

실행중 다음과 같은 변수가 채워집니다. 당신은 설정 변수로 가져올 수 있습니다.

* `hydra.job.name`: 작업 이름, 기본값으로 접두사 없이 파일이름을 가져옵니다. 해당 변수는 재정의할 수 있습니다.

* `hydra.job.override_dirname` : 이 작업을 위해 재정의에서 파생된 경로이름

* `hydra.job.num` : 스위프(sweep) 중인 작업 일련 번호

* `hydra.job.id` : 기본 작업 시슽메의 작업 ID(슬럼 등)

* `hydra.job.num_jobs`: 이 스위프(sweep)에서 실행중인 작업 수