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
3. 별도 전달받은 `FW_Total_V*.bin` 파일 선택
4. Flash offset 확인 (Total 이미지 : `0x0`)
5. Connect & Flash 버튼 클릭 → COM 포트 선택 → 자동 flash
6. 완료 시 JIG 자동 재부팅

## 대상 / 이미지

| 항목 | 값 |
|:-:|:-:|
| 칩 | ESP32-WROOM-32E (ESP32, Xtensa dual-core) |
| Flash | 4MB (N4) |
| Total 이미지 offset | `0x0` (부트로더+파티션+앱 병합) |

> `FW_Total_V*.bin` 은 `esptool merge_bin` 으로 0x0 부터 병합한 전체 이미지라 offset `0x0` 에 그대로 flash 한다.
> ESP32 의 부트로더는 원래 `0x1000` 이지만, Total 이미지는 0x0 부터 패딩되어 있어 offset `0x0` 이 맞다.
> `FW_Update_*.bin` (앱만) 은 offset 이 달라 `0x0` flash 시 부팅 안됨 — 업로드 금지(파일명 필터로 차단).

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
