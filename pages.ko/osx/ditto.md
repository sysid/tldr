# ditto

> 파일 및 디렉토리를 복사합니다.
> 더 많은 정보: <https://keith.github.io/xcode-man-pages/ditto.1.html>.

- 원본 디렉토리의 내용을 대상 디렉토리의 내용으로 덮어쓰기:

`ditto {{경로/대상/소스_폴더}} {{경로/대상/대상_폴더}}`

- 복사 중인 모든 파일에 대해 터미널 창에 한 줄씩 출력:

`ditto -V {{경로/대상/소스_폴더}} {{경로/대상/대상_폴더}}`

- 원본 파일 권한을 유지하면서 지정된 파일 또는 디렉토리 복사:

`ditto -rsrc {{경로/대상/소스_폴더}} {{경로/대상/대상_폴더}}`