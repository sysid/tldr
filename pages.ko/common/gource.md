# gource

> Git, SVN, Mercurial 및 Bazaar 저장소의 애니메이션 트리 다이러그램을 렌더링.
> 시간이 지남에 따라 생성, 수정 또는 제거되는 파일 및 디렉터리르 보여줌.
> 더 많은 정보: <https://gource.io>.

- 디렉토리에서 gource를 실행 (저장소의 루트 디렉토리가 아닌 경우, 그곳에서 루트를 찾음):

`gource {{경로/대상/레포지토리}}`

- 사용자 정의 출력 해상도를 사용해, 현재 디렉터리에서 gource를 실행:

`gource -{{너비}}x{{높이}}`

- 애니메이션의 기간을 지정:

`gource -c {{시간_척도_승수}}`

- 매일 애니메이션에 표시되는 시간을 지정 (제공된 경우, -c와 결합):

`gource -s {{초}}`

- 전체 화면 모드 및 사용자 정의 배경색 사용:

`gource -f -b {{hex_색상_코드}}`

- 애니메이션 제목을 지정:

`gource --title {{제목}}`