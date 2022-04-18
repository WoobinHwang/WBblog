---
title: "Kibana 설치"
author: "woobin"
date: '2022-04-18'
---

# 0417_kibana_install

- 설명: Elasticsearch 데이터를 시각화하고 Elastic Stack을 탐색하게 해주는 무료 오픈소스 인터페이스

- kibana 설치

```bash
$ sudo apt-get install kibana
```

![Untitled](/Images/0418_kibana_install/Untitled.png)

- kibana 서비스를 활성화

```bash
$ sudo systemctl enable kibana
```

![Untitled](/Images/0418_kibana_install/Untitled%201.png)

- 서비스를 시작하고, 확인

```bash
$ sudo systemctl start kibana
$ sudo systemctl status kibana
```

![Untitled](/Images/0418_kibana_install/Untitled%202.png)

- 로컬에서 확인
**[http://localhost:5601/](http://localhost:5601/)**

![Untitled](/Images/0418_kibana_install/Untitled%203.png)

- 끝났으면 서비스를 중지한다.

```bash
$ sudo systemctl stop kibana
```

- 끝