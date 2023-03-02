## 정보보호의 개념

정보의 수집·가공·저장·검색· 송신 중에 정보의 훼손·변조 · 유출 등을 방지하기 위한 관리적·기술적 수단, 또는 그러한 수단으로 이루어지는 행위

### 기업에서 정보 보호의 대상

출입하는 모든 사람 + 유무형의 정보 자산

## 목차

1. 사이트 보안
2. 웹 서버 보안
3. 웹 방화벽(WAF)

## 사이트 보안

### 파일 업로드 취약점

게시판 등의 첨부파일 기능을 이용해 허가되지 않은 파일들을 웹서버로 업로드할 수 있는 취약점 (php, jsp, asp, cgi, js, py 등)

### 파일 업로드 취약점 예방

파일 업로드 디렉토리 "실행" 권한 제거
허용되지 않은 확장자 업로드 제한

`httpd.conf <Directory "/usr/local/apache"> AllowOverride FileInfo </Directory> .htaccess <FilesMatch "\.(ph|inc|lib)"> Order allow, deny Deny from all </FilesMatch> AddType text/html html.htm.php .php3 -php4.phtml.phps in .c gㅑ -pl .shtml-jsp`

### XSS(Cross Site Scripting)

홈페이지 접속자의 권한 정보를 탈취하거나 악성코드 감염을 유발할 수 있는 취약점

### XSS(Cross Site Scripting) 예방

XSS 필터 라이브러리 사용

X-XSS-Protection 응답 헤더 사용

문자열 치환(<>& "등을 < > & " 로 치환)

**XSS Filter Library**

ESAPI

- XSS등 웹어플리케이션 시큐어
- 코딩 라이브러리
- 문자열 기반 유효성 검사

Lucyxss filter

- 자바 서블릿 기반 필터
- XML 설정만으로 세팅 가능

### SQL Injection

게시판 회원가입 창, URL 등을 통해 부적절한 값을 삽입하여 DB 데이터를 빼내거나 로그인 절차 우회 등 비정상 동작을 유발

### SQL Injection 예방

시큐어 코딩('--# /**/ 등 입력내용 필터링)
웹방화벽(Web Application Firewall, WAF) 구축

`PreparedStatement : 값을 바인딩하는 시점에서 전달된 특수문자 쿼리 등을 필터링하여 sql injection을 막는다. 1 preparedStatement = "SELECT * FROM users WHERE name = '" + userName + "';"; 2 3 # If someone put 4' or '1'='1 5 6 # Result 7 SELECT * FROM users WHERE name = '' OR '1'='1';`

X

`1 preparedStatement = "SELECT * FROM users WHERE name = ?"; 2 preparedStatement.setString(1, userName); 3 4 # If someone puts 5' or '1'='1 6 7 # Result 8 SQL FIND name : ' or '1'='1`

O

## 웹 서버 보안

- 계정 관리
- root 계정의 PATH 환경변수 설정
- 서비스 관리

### 계정관리

root 계정 원격 접속 제한

로그인 실패 임계값 설정

패스워드 복잡도 설정

### Root 계정 원격 접속 제한

`Telnet 원격접속 차단 1 vi /etc/securetty 2 pts/0~ pts/x 설정 제거 3 4 vi /etc/pam.d/login 5 # auth required /lib/security/pam_securetty.so #제거(주석제거) 6 auth required/lib/security/pam_securetty.so SSH 원격접속 차단 1 vi /etc/ssh/sshd_config 2 "PermitRootLogin no" 설정`

### 서비스 관리

Apache, Nginx 등 잘못된 보안 설정으로 발생할 수 있는 비인가자의 원격접 정보 노출 등을 제한하는 것을 목적으로 한다.

Directory Listing: 설정된 모든 Directory 옵션 지시자에서 Indexes 제거

`#Directory Listing Index of / Name Last modified Size Description secret/ 2017-01-27 15:40 priv/ 2017-01-27 15:41 edit/ 2017-01-27 15:40 dirl/ 2017-01-27 15:40 config.php 2017-01-27 15:40 11K Apache/2.4.23 (Wm64) PHP/5.6.25 Server at localhost Port 80`

`#httpd.conf 1 <Directory /> 2 #Options Indexes 3 AllowOverride None 4 Order allow, deny 5 Allow from all 6</Directory> 7`

### 웹 방화벽(WAF)

L7(OSI 7 Layers)에서 보안 유지

SQL Injection, XSS 등 웹 공격 탐지, 차단

DDOS, IP 차단, Rate limit 등 규칙 생성
