# uuencode

> 바이너리 파일을 ASCII로 인코딩하여 단순 ASCII 인코딩만 지원하는 매체를 통해 전송.
> 더 많은 정보: <https://manned.org/uuencode>.

- 파일을 인코딩하여 `stdout`에 결과 출력:

`uuencode {{경로/대상/입력_파일}} {{디코딩_후_출력_파일_이름}}`

- 파일을 인코딩하여 결과를 파일에 저장:

`uuencode -o {{경로/대상/출력_파일}} {{경로/대상/입력_파일}} {{디코딩_후_출력_파일_이름}}`

- 기본 uuencode 인코딩 대신 Base64를 사용하여 파일을 인코딩하고 결과를 파일에 저장:

`uuencode -m -o {{경로/대상/출력_파일}} {{경로/대상/입력_파일}} {{디코딩_후_출력_파일_이름}}`