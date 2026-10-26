# ESP32-WROOM-32E Web Downloader

ESP32-WROOM-32E-N4 (4MB Flash) 용 브라우저 기반 펌웨어 다운로더.
ESP32-P4-Web-Downloader 를 참고해 WROOM-32E-N4 버전으로 구성.

## 사용 URL

```
https://htk9998.github.io/ESP32-WROOM-32E-Web-Downloader/
```

## 사용 방법

1. JIG 를 USB-A 데이터 케이블로 PC 에 연결
2. 위 URL 을 Chrome / Edge 로 접속
3. bin 파일 선택 — `FW_Total_V*.bin`(전체) 또는 `FW_Update_*.bin`/앱
4. 이미지 종류 선택 → offset 자동 (Total `0x0` / App `0x10000` / 사용자지정)
5. Connect & Flash 버튼 클릭 → COM 포트 선택 → 자동 flash
6. 완료 시 JIG 자동 재부팅

## 대상 / 이미지

| 항목 | 값 |
|:-:|:-:|
| 칩 | ESP32-WROOM-32E (ESP32, Xtensa dual-core) |
| Flash | 4MB (N4) |
| Total 이미지 (`FW_Total_*.bin`) | offset `0x0` (부트로더+파티션+앱 병합) |
| App 이미지 (`FW_Update_*.bin`) | offset `0x10000` (앱만) |

- `FW_Total_V*.bin` : `esptool merge_bin` 으로 0x0 부터 병합한 전체 이미지 → offset `0x0`.
  ESP32 부트로더는 원래 `0x1000` 이지만 Total 은 0x0 부터 패딩돼 있어 `0x0` 이 맞다.
- `FW_Update_*.bin` (앱만, = IDF 빌드의 `epd_esp32.bin`) → offset `0x10000` (factory app 파티션).
- 파일 선택 시 이름으로 종류를 자동 판별해 offset 을 맞춰주고, 종류↔offset 이 어긋나면
  (0x0 에 앱만, 0x10000 에 전체) **확인창으로 경고**해 부팅 불능 실수를 막는다.

## 브라우저 요구사항

| 브라우저 | 지원 |
|:-:|:-:|
| Chrome 89+ | 지원 |
| Edge 89+ | 지원 |
| Opera 76+ | 지원 |
| Firefox | 미지원 (WebSerial 없음) |
| Safari | 미지원 (WebSerial 없음) |

## 기술 스택

- HTML5 + Vanilla JS (프레임워크 없음)
- esptool-js v0.4.5 (CDN : jsdelivr)
- WebSerial API (chip 통신, 칩 자동 감지)

## GitHub Pages 배포

저장소 Settings → Pages → Source 를 `main` / `/ (root)` 로 지정하면
위 URL 로 자동 HTTPS 서비스된다. (WebSerial 은 HTTPS 또는 localhost 필수)
