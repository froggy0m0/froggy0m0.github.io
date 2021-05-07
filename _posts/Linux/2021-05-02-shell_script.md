---
layout: post
title: 쉘 스크립트 사용
categories: Linux
message: 쉘 스크립스 사용에대해..


---

# 쉘 스크립트

​       

1. bash 쉘

   - test1

     ```
     #!/bin/bash
     python test.py
     ```

     1. test.py 를 포함하는 run.sh파일을 만든다

     2. run.sh를 실행 가능한 파일로 만든다. `$chomod +x run.sh `

     3. `$./run.sh` 실행

        

   - test2

     1. test.py 파일첫줄에 `#!/usr/bin/env python` (환경변수 설정 ?)

     2. test.py를 실행 가능한 파일로 만든다. `$chomod +x test.py`

     3. `$./test.py` 실행

         