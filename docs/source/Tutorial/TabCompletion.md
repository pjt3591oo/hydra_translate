# 탭 자동완성

다음과 같이 쉘 TAB 이용을 활성화할 수 있습니다.

```base
eval "$(python my_app.py -sc install=SHELL_NAME)"
```

SHELL_NAME을 당신의 쉘 이름으로 바꿉니다. 현재는 Bash만 지원하며 추가 쉘을 위한 자동완성 플러그인을 구현하기 위해 커뮤니티에 의존하고 있습니다.

Tab 완성은 설정그룹, 설정된 구성 노드와 값으로 시작하는 경우 `.` 또는 `/` 시작으로 완성할 수 있습니다.

탭 완성의 데모영상을 보아라.

[영상](https://asciinema.org/a/272604)
