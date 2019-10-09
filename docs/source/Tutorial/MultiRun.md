# 다중실행

가끔 당신은 파라미터 변경을 원합니다. 파라미터를 변경하기 위해 `--multirun`(`-m`)을 사용합니다. 그리고 당신이 전달하고자 하는 파라미터 값을 컴마(,)로 구분하여 리스트로 전달할 수 있습니다.

이것은 데이터베이스 타입(mysql, postresql)과 스키마(warehouse, support, school)을 변경하는 방법입니다. 출력은 설정의 구성을 포함하지 않습니다.

```bash
$ python tutorial/50_composition/my_app.py schema=warehouse,support,school db=mysql,postgresql -m
[2019-10-01 14:44:16,254] - Launching 6 jobs locally
[2019-10-01 14:44:16,254] - Sweep output dir : multirun/2019-10-01/14-44-16
[2019-10-01 14:44:16,254] -     #0 : schema=warehouse db=mysql
[2019-10-01 14:44:16,321] -     #1 : schema=warehouse db=postgresql
[2019-10-01 14:44:16,390] -     #2 : schema=support db=mysql
[2019-10-01 14:44:16,458] -     #3 : schema=support db=postgresql
[2019-10-01 14:44:16,527] -     #4 : schema=school db=mysql
[2019-10-01 14:44:16,602] -     #5 : schema=school db=postgresql
```

기본적인 방식은 로컬및 직력로 실행합니다.

AWS 위에 있는 코드를 동작할 수 있도록 런처에 추가할 계획이 있습니다.

