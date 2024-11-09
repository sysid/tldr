# last

> 마지막으로 로그인한 사용자 보기.
> 더 많은 정보: <https://manned.org/last>.

- `/var/log/wtmp`에서 읽어들인 마지막 로그인, 지속 시간 및 기타 정보 보기:

`last`

- 표시할 마지막 로그인 수 지정:

`last -n {{로그인_수}}`

- 항목의 전체 날짜와 시간을 출력하고 호스트 이름 열이 잘리지 않도록 마지막에 표시:

`last -F -a`

- 특정 사용자의 모든 로그인 보기 및 호스트 이름 대신 IP 주소 표시:

`last {{사용자명}} -i`

- 모든 기록된 재부팅 보기 (즉, 가상 사용자 "reboot"의 마지막 로그인):

`last reboot`

- 모든 기록된 종료 보기 (즉, 가상 사용자 "shutdown"의 마지막 로그인):

`last shutdown`