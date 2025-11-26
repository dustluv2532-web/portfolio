#  채팅앱 취약점 분석 및 보안 개선 시연

**기간:** 2025.09.22 ~ 2025.10.02  
**기술 스택:** Android(Java), Flask API, SQLite, JWT, ngrok HTTPS Tunnel, Wireshark

---

##  프로젝트 한 줄 요약
“Android 클라이언트 + Flask 서버 기반 채팅앱의 취약점을 분석하고,  
평문 전송 취약점 → TLS 적용 + JWT 인증으로 개선하여 민감 데이터 노출을 차단한 프로젝트”

---

##  목표 및 역할

### ✔ 역할
- 서버(Flask), Android 앱(자바) 개발  
- 취약점 분석 및 시연  
- TLS 적용 후 개선점 검증  

### ✔ 목표
1. **평문 전송을 의도적으로 구현해 취약점 증명**
2. Wireshark 스니핑으로 데이터 노출 재현  
3. **TLS 적용 + JWT 인증 절차 재설계**  
4. 개선된 환경에서 패킷 보안성 비교

---

##  아키텍처 구성도

![아키텍처](https://raw.githubusercontent.com/dustluv2532-web/portfolio/main/images/chat_arch.png)

**구성 설명:**  
- Android App → ngrok(HTTPS 터널) → Flask 서버  
- 서버는 JWT 기반 인증 후 메시지 처리  
- SQLite DB에 메시지를 기록  
- 개선 전/후 패킷 흐름 비교 가능

---

##  취약점 진단 및 시연

### ✔ 1) 평문 전송 취약점

앱에서 전송된 메시지가 **암호화 없이 그대로 노출됨**  
Wireshark 스니핑 결과, 입력한 채팅 내용이 평문으로 확인됨.

####  채팅앱 화면 & 평문 패킷
![채팅앱 화면](https://raw.githubusercontent.com/dustluv2532-web/portfolio/main/images/chat_app.png)
![평문 패킷](https://raw.githubusercontent.com/dustluv2532-web/portfolio/main/images/plain_packet.png)

> 전송되는 모든 메시지가 평문으로 노출 → 제3자가 쉽게 도청·복호화 가능

---

### ✔ 2) 단순 인증 로직으로 인한 보안성 부족
- 인증·세션 관리가 미흡  
- 토큰 없이 단순 요청만으로 메시지 송수신 가능  
- 재전송 공격(replay attack) 위험 존재

---

## 🔧 보안 개선 (TLS + JWT)

### ✔ 1) TLS 적용으로 암호화 전송 보장  
- HTTP → HTTPS 전환  
- 중간자 공격 및 패킷 스니핑 방지  

### ✔ 2) JWT 인증 구조 적용  
- 로그인 시 JWT 발급  
- 이후 모든 요청은 Authorization 헤더 기반으로 처리  
- 토큰 만료·재발급·리프레시 정책 실험

---

##  개선된 패킷 결과

TLS 적용 후 Wireshark에서 **메시지 내용이 완전 암호화됨**

![암호화된 패킷](https://raw.githubusercontent.com/dustluv2532-web/portfolio/main/images/encrypted_packet.png)

> 패킷 구조는 그대로 전달되지만, 데이터(payload)는 모두 암호화되어 확인 불가

---

##  프로젝트 결과

- 평문 전송 취약점 완전 제거  
- JWT + HTTPS 조합으로 인증 및 데이터 보호 강화  
- 재전송 공격 가능성 감소  
- 클라이언트–서버 구조와 패킷 전송 방식에 대한 이해도 향상

---

##  알게 된 점

- **JWT 인증 토큰의 역할과 리프레시 정책의 중요성**  
- 평문 전송이 얼마나 큰 보안 위험인지 실제 패킷을 통해 체감  
- HTTPS/TLS 적용 시 패킷 구조는 유지되지만 내용만 암호화되는 구조 학습  
- 포트폴리오 및 발표 자료에서 **민감한 데이터는 반드시 마스킹**해야 한다는 윤리적 책임 인식  
- 클라이언트–서버–패킷의 end-to-end 흐름을 와이어샤크로 직접 확인하며 전체 구조를 깊게 이해


