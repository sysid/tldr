# shar

> 쉘 아카이브 생성 도구.
> 더 많은 정보: <https://www.gnu.org/software/sharutils/manual/sharutils.html>.

- 주어진 파일들을 포함하고 실행 시 해당 파일들을 추출하는 쉘 스크립트 생성:

`shar --vanilla-operation {{경로/대상/파일1 경로/대상/파일2 ...}} > {{경로/대상/아카이브.sh}}`

- 아카이브 내 파일들을 압축:

`shar --compactor {{xz}} {{경로/대상/파일1 경로/대상/파일2 ...}} > {{경로/대상/아카이브.sh}}`

- 모든 파일을 바이너리로 처리 (즉, 모든 것을 `uuencode`):

`shar --uuencode {{경로/대상/파일1 경로/대상/파일2 ...}} > {{경로/대상/아카이브.sh}}`

- 모든 파일을 텍스트로 처리 (즉, 아무것도 `uuencode`하지 않음):

`shar --text-files {{경로/대상/파일1 경로/대상/파일2 ...}} > {{경로/대상/아카이브.sh}}`

- 아카이브의 헤더 주석에 이름과 컷 마크 포함:

`shar --archive-name "{{내_파일}}" --cut-mark {{경로/대상/파일1 경로/대상/파일2 ...}} > {{경로/대상/아카이브.sh}}`