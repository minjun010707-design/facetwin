# 제3자 자산 고지

이 디렉터리의 `anatomy.glb` 는 제3자 저작물의 파생물입니다.
아래 고지는 배포물에 반드시 포함되어야 합니다.

---

All or portions of this licensed product (such portions are the "Software")
have been obtained under license from The Brigham and Women's Hospital, Inc.
and are subject to the following terms and conditions:

**SPL Head and Neck Atlas**
Surgical Planning Laboratory, Brigham and Women's Hospital / Harvard Medical School.
https://www.openanatomy.org/atlas-pages/atlas-spl-head-and-neck.html
3D Slicer License — https://github.com/Slicer/Slicer/blob/master/License.txt

---

## 확인한 조건 (3D Slicer License Part B)

| 조항 | 내용 | 이행 |
|---|---|---|
| §1 | 로열티 없는 비독점 라이선스. 사용·복제·**파생물 제작**·배포·재라이선스 허용 | — |
| §2(i) | **독점(proprietary) 프로그램에 편입 가능** | 상업 이용 근거 |
| §1(b) | 사본·재라이선스에 라이선스 전문과 위 도입문구를 포함할 것 | 이 파일 |
| §1(c) | 저작자 표시·저작권 고지 유지 | 이 파일 |
| §1(d) | 수정본은 수정본임을 명시하고 원본으로 오인시키지 말 것 | 아래 "변형 내역" |
| §4 | 연구 목적 설계이며 FDA 미승인. **임상 적용은 권장되지 않음** | 제품 포지셔닝과 일치 — 상담 시각화 도구이며 진단·치료 결정에 쓰지 않음 |
| §6 | Brigham 및 기여자의 명칭·로고를 제품 홍보에 사용 금지 | 출처 표기 외 사용 안 함 |
| §3 | 제3자 권리는 별도. 원본 영상은 OsiriX MANIX 데이터셋 | **⚠ 아래 참조 — 미해결** |

## ⚠ §3 원본 영상 권리 (2026-07-26 확인, 미해결)

SPL 아틀라스는 OsiriX DICOM 라이브러리의 **MANIX** CT(256×256 축소본)에서 만들어졌습니다.
OsiriX 측 약관은 다음과 같습니다.

> These datasets are exclusively available for research and teaching.
> You are not authorized to redistribute or sell them, or use them for
> commercial purposes.
> — https://www.osirix-viewer.com/resources/dicom-image-library/

Brigham 은 Slicer License Part B 로 상업 이용까지 허용했지만, **§3 에서 제3자 권리는
별도라고 명시**합니다. 즉 Brigham 이 줄 수 없는 권리입니다.

따라서 다음 두 가지는 위 약관과 충돌할 수 있습니다.

1. **공개 배포** (GitHub Pages 등) = redistribute
2. **상업적 사용** = commercial purposes

`anatomy.glb` 는 CT 영상 자체가 아니라 그것을 분할해 만든 **표면 메쉬**이므로 파생물
해당 여부는 법적으로 다툼의 여지가 있으나, **안전하지 않습니다.**

**결론: 이 에셋은 개발·검증용입니다. 제품화 전에 상업 이용이 명시적으로 허용된
자산으로 교체해야 합니다** (BodyParts3D·Z-Anatomy = CC BY-SA, 또는 TCIA 의 CC-BY
두경부 CT 를 직접 분할). `.claude/build_anatomy.py` 파이프라인은 그대로 재사용됩니다.
(법률 자문이 아니며, 실제 배포 전 검토가 필요합니다.)

## 변형 내역 (§1(d))

원본 VTK 모델을 `.claude/build_anatomy.py` 가 다음과 같이 변형했습니다.

1. RAS(우/전/상) → 앱 좌표(우/상/전) 축 재배열
2. 정중시상면을 x=0 으로 보내는 강체 정렬 (기울기 3.87°, 오프셋 −2.8mm 보정)
3. 정점 클러스터링 데시메이션 (뼈 1.0~1.8mm, 연부 0.6~0.8mm 격자)
4. 외피 추출 — 바깥에서 보이지 않는 내부면(부비동·두개저·내판) 제거
5. 59개 모델 중 안면 성형과 무관한 49개(경추·늑골·혈관·갑상선 등) 제외
6. glTF 2.0 바이너리(GLB)로 재포장

즉 **형상 자체는 원본이고, 좌표계·해상도·포함 범위만 바꾼 것**입니다.
치수를 왜곡하는 변형(스케일·비균등 변형)은 하지 않았습니다.
