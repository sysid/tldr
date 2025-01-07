# kill

> 프로세스에 신호를 보내는 유틸리티로, 주로 프로세스 중지와 관련이 있습니다.
> SIGKILL 및 SIGSTOP을 제외한 모든 신호는 프로세스에 의해 가로채져서 안전하게 종료될 수 있습니다.
> 더 많은 정보: <https://manned.org/kill>.

- 기본 SIGTERM (종료) 신호를 사용하여 프로그램 종료:

`kill {{프로세스_ID}}`

- 신호 값과 해당 이름 목록 표시 (`SIG` 접두사를 제외하고 사용):

`kill -L`

- 백그라운드 작업 종료:

`kill %{{작업_ID}}`

- SIGHUP (연결 끊김) 신호를 사용하여 프로그램 종료. 많은 데몬이 종료 대신 다시 로드됩니다:

`kill -{{1|HUP}} {{프로세스_ID}}`

- SIGINT (인터럽트) 신호를 사용하여 프로그램 종료. 일반적으로 사용자가 `Ctrl + C`를 누를 때 시작됩니다:

`kill -{{2|INT}} {{프로세스_ID}}`

- 운영 체제에 프로그램을 즉시 종료하도록 신호 (프로세스가 신호를 캡처할 기회가 없음):

`kill -{{9|KILL}} {{프로세스_ID}}`

- 운영 체제에 SIGCONT ("계속") 신호를 받을 때까지 프로그램을 일시 중지하도록 신호:

`kill -{{17|STOP}} {{프로세스_ID}}`

- 주어진 GID (그룹 ID)를 가진 모든 프로세스에 `SIGUSR1` 신호 보내기:

`kill -{{SIGUSR1}} -{{그룹_ID}}`