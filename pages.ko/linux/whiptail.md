# whiptail

> 셸 스크립트에서 텍스트 기반의 대화 상자를 표시합니다.
> 더 많은 정보: <https://manned.org/whiptail>.

- 간단한 메시지 표시:

`whiptail --title "{{제목}}" --msgbox "{{메시지}}" {{문자_높이}} {{문자_너비}}`

- 불리언 선택을 표시하고 종료 코드를 통해 결과 반환:

`whiptail --title "{{제목}}" --yesno "{{메시지}}" {{문자_높이}} {{문자_너비}}`

- 예/아니오 버튼의 텍스트 사용자 지정:

`whiptail --title "{{제목}}" --yes-button "{{텍스트}}" --no-button "{{텍스트}}" --yesno "{{메시지}}" {{문자_높이}} {{문자_너비}}`

- 텍스트 입력 상자 표시:

`{{결과_변수_이름}}="$(whiptail --title "{{제목}}" --inputbox "{{메시지}}" {{문자_높이}} {{문자_너비}} {{기본_텍스트}} 3>&1 1>&2 2>&3)"`

- 비밀번호 입력 상자 표시:

`{{결과_변수_이름}}="$(whiptail --title "{{제목}}" --passwordbox "{{메시지}}" {{문자_높이}} {{문자_너비}} 3>&1 1>&2 2>&3)"`

- 다중 선택 메뉴 표시:

`{{결과_변수_이름}}=$(whiptail --title "{{제목}}" --menu "{{메시지}}" {{문자_높이}} {{문자_너비}} {{메뉴_표시_높이}} "{{값_1}}" "{{표시_텍스트_1}}" "{{값_n}}" "{{표시_텍스트_n}}" ..... 3>&1 1>&2 2>&3)`