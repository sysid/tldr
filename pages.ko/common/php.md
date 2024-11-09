# php

> PHP 명령줄 인터페이스.
> 더 많은 정보: <https://php.net>.

- PHP 스크립트를 구문 분석하고 실행:

`php {{경로/대상/파일}}`

- PHP 스크립트의 문법 검사(즉, 린트):

`php -l {{경로/대상/파일}}`

- PHP를 대화형으로 실행:

`php -a`

- PHP 코드 실행(참고: `<? ?>` 태그를 사용하지 마세요; 큰따옴표는 백슬래시로 이스케이프하세요):

`php -r "{{코드}}"`

- 현재 디렉토리에서 PHP 내장 웹 서버 시작:

`php -S {{호스트:포트}}`

- 설치된 PHP 확장 목록:

`php -m`

- 현재 PHP 구성에 대한 정보 표시:

`php -i`

- 특정 함수에 대한 정보 표시:

`php --rf {{함수_이름}}`