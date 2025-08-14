# 💽 홈 서버 구축
> 사용하지 않는 노트북으로 서버를 구축하여 네트워크 구조에 대한 이해를 높힘  
> 2025.08.10 ~ 진행 중

<br>

## 📑 목차
- [서버 스펙](#-서버-스펙)
- [기술 스택](#-기술-스택)
- [개발 과정](#-개발-과정)
- [트러블슈팅](#-트러블슈팅)

<br>

## 💻 서버 스펙

| 품목    | 이름                          |
|:------:|:----------------------------:|
| 노트북   | LG전자 15U530-LH10K          |
| CPU    | Intel Pentium 3556U(1.7GHz) |
| RAM    | 4GB                         |
| 스토리지 | HDD 500GB                   |

<br>

## 🧱 기술 스택

| 항목       | 버전/도구                       |
|:---------:|:-----------------------------:|
| OS        | Ubuntu Desktop 24.04.3 LTS    |
| Web Proxy | Nginx                         |
| Security  | UFW, Fail2ban                 | 

<br>

## 🔍 개발 과정
1. [OS 설치](https://gym-developer.tistory.com/182)
2. [SSH, UFW, 포트포워딩](https://gym-developer.tistory.com/183)
3. [보안 강화 및 Nginx 설치](https://gym-developer.tistory.com/184)
4. [도메인 연결 및 HTTPS 설정](https://gym-developer.tistory.com/187)

<br>

## 🏹 트러블슈팅

### SSH 접속 이슈
<details>
  <summary> 자세히 보기 </summary>
  
  - 문제 : 외부에서 SSH 접속 시 서버와 호스트 간 호스트 키 타입 불일치로 핸드셰이크 실패
  - 원인  
    - 서버 외부 포트 22는 공유기/모뎀 SSH가 응답 중 → 서버의 ed25519 키 제공 불가.  
    - 포트포워딩이 HTTP(80)만 되어 있고, SSH 포트 포워딩 없음.  
    - 서버에서 ed25519 키는 존재하지만, 외부에선 ssh-rsa만 보임.  
    - 방화벽(UFW)에서 2222 포트 차단.  
    - 접속 성공 후에도 비밀번호 로그인 활성화 → 공개키 인증 미적용.  
  - 해결
    - 서버 키 타입 문제 해결 : 서버 /etc/ssh/sshd_config에 ed25519 키 경로 추가 및 활성화
    - 포트포워딩 수정 : 외부 2222 → 내부 2222 구조
    - 서버 SSH 포트 다중 리슨 구성
    
</details>
