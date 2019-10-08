# 설정파일 경로

히드라는 객체로 구성된 설정 파일을 찾기위해 경로 접근방식을 사용합니다. `SearchPathPlugin`은 검색경로를 제어할 수 있습니다.

히드라 로거의 세부 로깅을 켜서 히드라에 의해 로드된 검색 경로와 구성을 검사할 수 있습니다.

```bash
$ python my_app.py hydra.verbose=hydra
```