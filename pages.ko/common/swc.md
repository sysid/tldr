# swc

> Rust로 작성된 JavaScript 및 TypeScript 컴파일러.
> 더 많은 정보: <https://swc.rs>.

- 지정된 입력 파일을 변환하여 `stdout`에 출력:

`swc {{경로/대상/파일}}`

- 입력 파일이 변경될 때마다 변환:

`swc {{경로/대상/파일}} --watch`

- 지정된 입력 파일을 변환하여 특정 파일에 출력:

`swc {{경로/대상/입력_파일}} --out-file {{경로/대상/출력_파일}}`

- 지정된 입력 디렉토리를 변환하여 특정 디렉토리에 출력:

`swc {{경로/대상/입력_폴더}} --out-dir {{경로/대상/출력_폴더}}`

- 특정 설정 파일을 사용하여 지정된 입력 디렉토리를 변환:

`swc {{경로/대상/입력_폴더}} --config-file {{경로/대상/.swcrc}}`

- glob 경로를 사용하여 지정된 디렉토리의 파일 무시:

`swc {{경로/대상/입력_폴더}} --ignore {{경로/대상/무시할_파일1 경로/대상/무시할_파일2 ...}}`