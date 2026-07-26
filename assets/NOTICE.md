# 제3자 자산 고지

이 디렉터리의 `anatomy.glb` 는 제3자 저작물의 파생물입니다.
아래 고지는 배포물에 반드시 포함되어야 합니다.

---

**BodyParts3D**
Copyright © 2008 Life Science Integrated Database Center (DBCLS), Japan
licensed by CC Display-Inheritance 2.1 Japan
(= Creative Commons Attribution-ShareAlike 2.1 Japan, CC BY-SA 2.1 JP)

- 원본  https://lifesciencedb.jp/bp3d/
- 라이선스 전문  https://creativecommons.org/licenses/by-sa/2.1/jp/
- 이 빌드가 쓴 STL 변환본
  https://github.com/Kevin-Mattheus-Moerman/BodyParts3D (동일 라이선스로 재배포)

**`anatomy.glb` 는 CC BY-SA 2.1 Japan 으로 배포됩니다.**

---

## 확인한 조건 (CC BY-SA 2.1 JP)

| 조항 | 내용 | 이행 |
|---|---|---|
| BY | 원저작자 표시 | 이 파일 · 앱 화면의 출처 표기 |
| SA | 파생물을 **동일 라이선스**로 배포 | `anatomy.glb` 를 CC BY-SA 2.1 JP 로 배포 (아래 참조) |
| — | 상업 이용 제한 **없음** | 제품 투입 가능 |
| — | 개작 제한 **없음** | 데시메이션·좌표 변환·TPS 변형 가능 |
| §4(a) | 기술적 보호조치로 이용을 제한하지 말 것 | GLB 는 암호화·DRM 없이 그대로 제공 |

## ShareAlike 를 어떻게 지키는가 (중요)

CC BY-SA 는 카피레프트입니다. **파생물**은 같은 라이선스로 배포해야 합니다.
그래서 이 프로젝트는 다음과 같이 경계를 둡니다.

- `assets/anatomy.glb` = 파생물 → **CC BY-SA 2.1 JP**
- `index.html` 등 애플리케이션 코드 = 파생물이 **아님** → 프로젝트 자체 라이선스

근거: 앱 코드는 BodyParts3D 의 형상을 담고 있지 않고, 실행 시점에 GLB 파일을
**불러다 쓸 뿐**입니다(집합저작물 aggregation). 형상 데이터를 코드에 하드코딩하거나
빌드 산출물 안에 인라인으로 굽는 순간 이 경계가 무너지므로, `anatomy.glb` 는 반드시
**별도 파일로 유지**해야 합니다.

같은 이유로 다음은 하면 안 됩니다.

- GLB 를 base64 로 `index.html` 안에 인라인
- 번들러가 에셋을 JS 로 인라인하도록 설정
- 이 메쉬에서 뽑은 형상 통계(예: 통계형상모델 계수)를 코드에 박아 넣기

(법률 자문이 아닙니다. 실제 배포 전 검토가 필요합니다.)

## 왜 SPL Head and Neck Atlas 를 버렸는가

이전 빌드는 SPL Head and Neck Atlas(Brigham and Women's Hospital, 3D Slicer License)를
썼습니다. Slicer License Part B 는 상업 이용까지 허용하지만, **§3 에서 제3자 권리는
별도**라고 명시합니다. SPL 아틀라스의 원본 영상은 OsiriX DICOM 라이브러리의 MANIX CT 이고,
OsiriX 약관은 이렇습니다.

> These datasets are exclusively available for research and teaching.
> You are not authorized to redistribute or sell them, or use them for
> commercial purposes.
> — https://www.osirix-viewer.com/resources/dicom-image-library/

즉 Brigham 이 줄 수 없는 권리였습니다. 공개 배포(redistribute)와 상업적 사용이 둘 다
막히므로 제품에 쓸 수 없었습니다. 덤으로 SPL 에는 표정근이 아예 없어서(측두근·교근·
관골근 + 목 근육이 전부) 보톡스·팔자주름 같은 핵심 시술을 표현할 수 없었습니다.

BodyParts3D 로 옮기면서 두 문제가 한꺼번에 풀렸습니다.

## 포함 범위

| 구분 | 개수 | 비고 |
|---|---|---|
| 두개골 뼈 | 20 | 전두·후두·접형·사·서골 + 측두·두정·관골·누·비·상악·구개·하비갑개 (좌우) |
| 상·하악 치아 | 28 | 절치·견치·소구치·대구치 |
| 하악골 | 1 | 절골로 따로 움직이므로 분리 |
| 저작근 | 3쌍 | 측두근 · 교근 천부 · 교근 심부 |
| 표정근 | 17쌍 + 구륜근 | 전두·추미·비근근·안륜근(안와부/검부)·비근·비중격하체근·상순비익거근·상순거근·대관골근·소관골근·구각거근·소근·구각하제근·하순하제근·이근·협근 |

침샘·혈관·신경은 넣지 않았습니다 — 근육 층에 두면 그 층이 무엇인지 흐려집니다.

## 변형 내역

원본 STL 을 `.claude/build_anatomy.py` 가 다음과 같이 변형했습니다.

1. binary STL 파싱 + 정점 용접 (STL 은 인덱스가 없어 정점이 3배로 중복됨)
2. DICOM LPS(좌/후/상) → 앱 좌표(우/상/전) 축 재배열
3. 정중시상면을 x=0 으로 보내는 강체 정렬 (기울기 0.10°, 오프셋 −1.5mm 보정)
   + 원점을 nasion 으로 이동
4. 정점 예산에 맞춘 클러스터링 데시메이션 (뼈 16000v/5000v, 근육 500~1500v)
5. 뼈만 외피 추출 — 바깥에서 보이지 않는 내부면(부비동·두개저·내판) 제거
6. glTF 2.0 바이너리(GLB)로 재포장, 계측점 19개를 `asset.extras` 에 기록

즉 **형상 자체는 원본이고, 좌표계·해상도·포함 범위만 바꾼 것**입니다.
치수를 왜곡하는 변형(스케일·비균등 변형)은 빌드 단계에서 하지 않았습니다.
런타임의 TPS 워프는 화면에서만 일어나고 이 파일에는 기록되지 않습니다.
