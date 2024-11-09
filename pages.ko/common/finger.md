# finger

> 사용자 정보조회 프로그램.
> 더 많은 정보: <https://manned.org/finger>.

- 현재 로그인한 사용자에 대한 정보 표시:

`finger`

- 특정 사용자에 대한 정보 표시:

`finger {{사용자명}}`

- 사용자의 로그인 이름, 실명, 단말기 이름 및 기타 정보를 표시:

`finger -s`

- `-s`와 동일한 정보는 물론 사용자의 홈 디렉터리, 집. ㅓㄴ화번호, 로그인 쉘, 메일 상태 등을 표시하는 여러 줄 출력 형식을 생성:

`finger -l`

- 사용자 이름과의 일치를 방지하고 로그인 이름만 사용:

`finger -m`