# mcs

> Mono C# 컴파일러.
> 더 많은 정보: <https://manned.org/mcs.1>.

- 지정된 파일 컴파일:

`mcs {{경로/대상/입력_파일1.cs 경로/대상/입력_파일2.cs ...}}`

- 출력 프로그램 이름 지정:

`mcs -out:{{경로/대상/파일.exe}} {{경로/대상/입력_파일1.cs 경로/대상/입력_파일2.cs ...}}`

- 출력 프로그램 유형 지정:

`mcs -target:{{exe|winexe|library|module}} {{경로/대상/입력_파일1.cs 경로/대상/입력_파일2.cs ...}}`