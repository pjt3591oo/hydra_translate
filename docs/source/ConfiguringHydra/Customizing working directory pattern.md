# 작업 디렉터리 커스텀마이징 패턴

사용자 지정을 적용하는 방법에 대한 자세한 내용은 [소개](./introduction.md)를 참조하세요.

일별로 그룹화된 출력 디렉터리

```yaml
hydra:
  run:
    dir: ./outputs/${now:%Y-%m-%d}/${now:%H-%M-%S}
```

스위프(sweep) 하위 디렉터리는 작업 인스턴스에 대한 재정의 매개변수를 포함합니다.

```yaml
hydra:
  sweep:
    subdir: ${hydra.job.num}_${hydra.job.id}_${hydra.job.override_dirname}
```

작업 이름별로 그룹화된 출력 디렉터리

```yaml
hydra:
  run:
    dir: outputs/${hydra.job.name}/${now:%Y-%m-%d_%H-%M-%S}
```

출력 디렉터리는 사용자 변수를 포함할 수 있습니다.

```yaml
hydra:
  run:
    dir: outputs/${now:%Y-%m-%d_%H-%M-%S}/opt:${optimizer.type}
```

출력 디렉터리는 작업을 위해 재정의된 파라미터를 포함할 수 있습니다.

```yaml
hydra:
  run:
    dir: output/${hydra.job.override_dirname}
```