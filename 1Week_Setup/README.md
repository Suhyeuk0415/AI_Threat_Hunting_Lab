# 1주 차: 타겟 인프라 세팅 및 취약점 환경 구성

## 1. 인프라 구동 및 방화벽 설정
- **웹 서버 엔진**: Apache (httpd)
- **방화벽 정책**: HTTP(80) 포트 허용 및 서비스 구동 완료
<img width="651" height="531" alt="image" src="https://github.com/user-attachments/assets/658a3729-52ff-4059-9f29-b110054774ed" />


## 2. 취약한 백엔드 소스코드 작성 (LFI)
- 외부 입력값 검증이 누락된 PHP 코드를 구성하여 로컬 파일 포용(LFI) 취약점 환경을 구축했습니다.
<img width="655" height="531" alt="image" src="https://github.com/user-attachments/assets/1f717cf6-5f72-425f-876b-8cd56a8d62a1" />


## 3. LFI 취약점 발현 증명 (PoC)
- **공격 벡터**: `?page=/etc/passwd`
- **결과**: 서버의 시스템 계정 정보 파일이 브라우저에 그대로 노출됨을 확인했습니다. (향후 AI 툴의 탐지 여부 평가를 위한 대조군)
<img width="1009" height="633" alt="image" src="https://github.com/user-attachments/assets/e5f9b939-c361-4273-9129-b879853bd1d9" />


## 4. API 인가 우회(BOLA) 취약점 구현 및 증명
- 사용자 인증(Authorization) 로직이 누락된 API 엔드포인트(`api_user.php`)를 배포했습니다.
- **공격 벡터**: `?id=1` 파라미터 변조
- **결과**: 일반 사용자 권한에서 파라미터 조작만으로 관리자(Admin)의 민감한 JSON 데이터(시크릿 키) 탈취에 성공했습니다.
<img width="1011" height="628" alt="image" src="https://github.com/user-attachments/assets/c5361ed8-3f26-44bc-b15d-ed2907b4c420" />

## 💡 트러블슈팅 (Troubleshooting)
- **이슈**: CLI에서 GUI(GNOME) 모드로 패키지 전환 후, 리눅스의 디스플레이 서버(Wayland)와 VMware 그래픽 드라이버 간 충돌로 인해 로그인 화면 멈춤 현상 발생.
- **해결**: `Ctrl+Alt+F2`로 가상 콘솔(CLI)에 강제 진입하여 `/etc/gdm/custom.conf` 파일의 `WaylandEnable=false` 주석을 해제하고 충돌을 해결하여 정상 부팅 완료.
