---
title: "Oracle SQL 삭제"
author: "woobin"
date: '2022-04-25'
---

# 0425_Oracle_SQL_deinsert

- 오라클 설치중에 중지 시켜 전부 다 삭제하고 다시 설치를 시작해야 하는 상황인데 그냥 삭제하려니까 삭제가 안됨.

- 제어판에서 관리도구 선택

![Untitled](/Images/0425_Oracle_SQL_deinsert/Untitled.png)

- 서비스 메뉴를 선택하여 oracle로 시작하는 모든 서비스를 중지시킨다.

![Untitled](/Images/0425_Oracle_SQL_deinsert/Untitled%201.png)

- deinstall 폴더의 deinstall 파일을 관리자 권한으로 실행
필자의 경로
C:\sql_lecture\db_homes\WINDOWS.X64_193000_db_home\deinstall

![Untitled](/Images/0425_Oracle_SQL_deinsert/Untitled%202.png)

- 실행하면 명령프롬프트 창이 나오는데
엔터나 y만 누르다가 삭제 할 데이터베이스의 이름 목록을 적으라는 부분에서 데이터 베이스를 적어주고, 저장 영역 유형은 FS로 적어준다.
나머지는 y를 눌러 넘어가주면 됨

- 레지스트리 편집기를 켠다.

![Untitled](/Images/0425_Oracle_SQL_deinsert/Untitled%203.png)

- 1. HKEY_LOCAL_MACHINE - > SOFTWARE - > ORACLE로 시작하는 항목 삭제
2. HKEY_LOCAL_MACHINE - > SYSTEM - > ControlSet001 - > service - > ORACLE로 시작하는 항목 삭제
3. HKEY_LOCAL_MACHINE - > SYSTEM - > ControlSet002 - > service - > ORACLE로 시작하는 항목 삭제
4. HKEY_LOCAL_MACHINE - > SYSTEM - > CurrentControlSet - > service - > ORACLE로 시작하는 항목 삭제

- 컴퓨터를 재부팅하고 오라클 홈 디렉토리에 파일이 남아있다면 삭제하면 끝남.