# pdfcrop

> PDF 파일의 각 페이지에서 여백을 감지하고 제거.
> 더 많은 정보: <https://github.com/ho-tex/pdfcrop>.

- PDF 파일의 각 페이지에서 여백을 자동으로 감지하고 제거:

`pdfcrop {{경로/대상/입력_파일.pdf}} {{경로/대상/출력_파일.pdf}}`

- 각 페이지의 여백을 특정 값으로 설정:

`pdfcrop {{경로/대상/입력_파일.pdf}} --margins '{{왼쪽}} {{위쪽}} {{오른쪽}} {{아래쪽}}' {{경로/대상/출력_파일.pdf}}`

- 각 페이지의 여백을 동일한 값으로 설정 (왼쪽, 위쪽, 오른쪽, 아래쪽 모두 동일):

`pdfcrop {{경로/대상/입력_파일.pdf}} --margins {{300}} {{경로/대상/출력_파일.pdf}}`

- 자동 감지 대신 사용자 정의 경계 상자를 사용하여 자르기:

`pdfcrop {{경로/대상/입력_파일.pdf}} --bbox '{{왼쪽}} {{위쪽}} {{오른쪽}} {{아래쪽}}' {{경로/대상/출력_파일.pdf}}`

- 홀수 및 짝수 페이지에 대해 다른 사용자 정의 경계 상자 사용:

`pdfcrop {{경로/대상/입력_파일.pdf}} --bbox-odd '{{왼쪽}} {{위쪽}} {{오른쪽}} {{아래쪽}}' --bbox-even '{{왼쪽}} {{위쪽}} {{오른쪽}} {{아래쪽}}' {{경로/대상/출력_파일.pdf}}`

- 성능 향상을 위해 낮은 해상도로 여백 자동 감지:

`pdfcrop {{경로/대상/입력_파일.pdf}} --resolution {{72}} {{경로/대상/출력_파일.pdf}}`