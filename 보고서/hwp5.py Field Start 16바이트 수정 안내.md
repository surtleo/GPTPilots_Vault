# hwp5.py Field Start 16바이트 수정 안내

케빈랩 HWP 추출 타임아웃을 해결하기 위한 **로컬 가상환경 임시 수정** 안내입니다.

> 이 수정은 프로젝트 Git 코드가 아니라 각 컴퓨터의 `.venv`에 설치된 외부 라이브러리를 고치는 방식입니다. 가상환경을 다시 만들거나 라이브러리를 재설치하면 사라지므로, 팀원 전원이 같은 방법으로 적용해야 합니다.

## 1. 수정할 파일 열기

VS Code에서 아래 파일을 엽니다.

```text
GPTPilots_Project\.venv\Lib\site-packages\hwp_hwpx_parser\hwp5.py
```

수정 전에 파일을 복사해 `hwp5.py.bak`로 백업해 두는 것을 권장합니다.

## 2. 수정할 함수 찾기

파일에서 아래 함수를 검색합니다.

```python
def _extract_hyperlink_texts_from_para(self, para_data: bytes) -> List[str]:
```

그 함수 안에서 아래 블록을 찾습니다.

```python
if code == 0x03:
```

## 3. 어디부터 어디까지 지우나

아래 블록 전체를 지웁니다. 시작은 `if code == 0x03:`이고, 끝은 마지막 `i += 14` 다음의 `continue`입니다.

```python
if code == 0x03:
    if i + 6 <= len(para_data):
        ctrl_id = struct.unpack_from("<I", para_data, i + 2)[0]

        if ctrl_id == CTRL_ID_HYPERLINK:
            text_start = i + 14
            text_chars = []
            j = text_start

            while j < len(para_data) - 1:
                c = struct.unpack_from("<H", para_data, j)[0]
                if c == 0x04:
                    break
                elif c == 0x03:
                    j += 2
                elif 0x20 <= c < 0x10000:
                    text_chars.append(chr(c))
                    j += 2
                else:
                    j += 2

            if text_chars:
                hyperlink_texts.append("".join(text_chars))
            i = j
            continue
        else:
            i += 14
            continue
```

바로 다음의 아래 코드는 삭제하면 안 됩니다.

```python
elif code == 0x04:
```

## 4. 같은 위치에 붙여넣을 코드

삭제한 자리, 즉 `elif code == 0x04:` 바로 위에 아래 코드를 붙여넣습니다.

```python
if code == 0x03:
    field_start_size = 16

    if i + field_start_size > len(para_data):
        break

    ctrl_id = struct.unpack_from("<I", para_data, i + 2)[0]

    if ctrl_id == CTRL_ID_HYPERLINK:
        text_start = i + field_start_size
        text_chars = []
        j = text_start

        while j < len(para_data) - 1:
            c = struct.unpack_from("<H", para_data, j)[0]
            if c == 0x04:
                break
            elif c == 0x03:
                j += 2
            elif 0x20 <= c < 0x10000:
                text_chars.append(chr(c))
                j += 2
            else:
                j += 2

        if text_chars:
            hyperlink_texts.append("".join(text_chars))
        i = j
        continue
    else:
        i += field_start_size
        continue
```

최종 구조는 아래처럼 이어져야 합니다.

```python
if code == 0x03:
    # 위 수정 코드
elif code == 0x04:
    # 기존 코드 유지
```

## 5. 왜 16바이트인가

한컴 HWP 공식 문서에서 `0x0003`은 Field Start 확장 컨트롤이며, 확장 컨트롤 크기는 8 WCHAR입니다. 문단 텍스트에서 WCHAR는 2바이트이므로 총 16바이트를 소비해야 합니다.

기존 코드는 14바이트만 소비해 마지막 2바이트를 남겼고, 케빈랩 문서에서는 이 값이 다시 `03 00`으로 읽히면서 위치가 이동하지 않는 무한 루프가 발생했습니다.

## 6. 적용 확인

케빈랩 HWP를 다시 추출했을 때 다음처럼 나오면 적용 성공입니다.

```text
성공: True
추출 글자 수: 56,914
```

