---
title: "배포 - 파일 업로드(Markdown 형식)"
author: "woobin"
date: '2022-03-18'
---

# 파일 업로드(markdown 형식)

## R - > hexo

- R 상단 창 예시

```bash
---
title: "title_name"
author: "Woobin"
date: '2022-mm-dd'
output:
	html_document:
		keep_md: true
---
```

- knit 실행시  <파일명>.md,  <파일명>.html,  <파일명>_files 가 생성됨.
- source / _posts 폴더로 <파일명>.md 파일 옮기기
source / Images 폴더로 <파일명>_files 폴더 옮기기
- 이미지 파일이 있으면 md 파일에서 주소 설정 필요
ex)  /Images/ ~해당 이미지가 있는 주소명까지
    
    ![Untitled](/Images/0318_File_Upload(Markdown)/Untitled.png)
    
- hexo server로 확인해보기

## Notion - > hexo

![Untitled](/Images/0318_File_Upload(Markdown)/Untitled%201.png)

- 노션의 상단 ...에서 내보내기 (Markdown형식)
- 압축 해제 후
source / _posts 폴더로 <파일명>.md 파일 옮기기
source / Images 폴더로 <파일명>_files 폴더 옮기기
- md 파일에서 이미지 파일의 경로가 이상하게 지정되어 있어 바꿔야함.

![Untitled](/Images/0318_File_Upload(Markdown)/Untitled%202.png)

- Copy Path/Reference... 클릭

![Untitled](/Images/0318_File_Upload(Markdown)/Untitled%203.png)

- Path From Repository Root 선택
기본적으로 source는 잡혀있으니까 제외하고 /Images/~파일명 으로 교체
- hexo server 로 확인

## JupyterLab - > hexo

- anaconda를 관리자 권한으로 실행
- JupyterLab 실행
- Save and Export Notebook As...  - > Markdown으로 압축 후 바탕화면에서 압축해제

![Untitled](/Images/0318_File_Upload(Markdown)/Untitled%204.png)

- source / _posts 폴더로 <파일명>.md 파일 옮기기
source / Images 폴더로 <파일명>_files 폴더 옮기기
- 이미지 파일이 있으면 md 파일에서 주소 설정 필요
ex)  /Images/ ~해당 이미지가 있는 주소명까지

![Untitled](/Images/0318_File_Upload(Markdown)/Untitled%205.png)