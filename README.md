# Excalidraw 다이어그램 스킬

[English](README.en.md)

자연어 설명을 아름답고 실용적인 Excalidraw 다이어그램으로 변환하는 코딩 에이전트 스킬입니다. 단순한 박스-화살표 다이어그램이 아니라 **시각적으로 논증하는** 다이어그램을 생성합니다.

스킬을 지원하는 모든 코딩 에이전트와 호환됩니다. `.claude/skills/`에서 읽는 에이전트([Claude Code](https://docs.anthropic.com/en/docs/claude-code), [OpenCode](https://github.com/nicepkg/OpenCode) 등)의 경우, 해당 디렉토리에 넣기만 하면 바로 사용할 수 있습니다.

![루프 엔지니어링 14단계 로드맵 예시 다이어그램](samples/loop-engineering-14-step-roadmap.png)

> 위 다이어그램은 이 스킬로 생성한 예시 결과물입니다.

## 차별점

- **표시가 아닌 논증하는 다이어그램.** 모든 도형/도형 그룹이 표현하는 개념을 반영합니다 — 일대다 관계에는 팬아웃, 순서에는 타임라인, 집계에는 수렴. 일정한 카드 그리드는 없습니다.
- **증거 아티팩트.** 예시로, 기술 다이어그램에는 실제 코드 스니펫과 JSON 페이로드가 포함됩니다.
- **내장 시각적 유효성 검사.** Playwright 기반 렌더 파이프라인을 통해 에이전트가 자체 출력을 확인하고, 레이아웃 문제(텍스트 겹침, 화살표 정렬 오류, 불균형한 간격)를 발견하여 전달하기 전에 반복적으로 수정합니다.
- **브랜드 커스터마이징 가능.** 모든 색상과 브랜드 스타일은 단일 파일(`references/color-palette.md`)에 집중되어 있습니다. 이 파일을 교체하면 모든 다이어그램이 해당 팔레트를 따릅니다.

## 설치

이 저장소를 클론하거나 다운로드한 후 프로젝트의 `.claude/skills/` 디렉토리에 복사합니다:

```bash
git clone https://github.com/GyeongjinLee/gen-excalidraw-diagram-skill.git
cp -r gen-excalidraw-diagram-skill .claude/skills/gen-excalidraw-diagram-skill
```

## 설정

스킬에는 에이전트가 다이어그램을 시각적으로 검증할 수 있는 렌더 파이프라인이 포함되어 있습니다. 두 가지 설정 방법이 있습니다:

**옵션 A: 코딩 에이전트에게 요청 (가장 쉬운 방법)**

에이전트에게 다음과 같이 요청하면 됩니다: *"SKILL.md의 지침에 따라 Excalidraw 다이어그램 스킬 렌더러를 설정해줘."* 에이전트가 명령을 직접 실행합니다.

**옵션 B: 수동 설정**

```bash
cd .claude/skills/gen-excalidraw-diagram-skill/references
uv sync
uv run playwright install chromium
```

## 사용법

코딩 에이전트에게 다이어그램 생성을 요청합니다:

> "AG-UI 프로토콜이 AI 에이전트에서 프론트엔드 UI로 이벤트를 스트리밍하는 방식을 보여주는 Excalidraw 다이어그램을 만들어줘"

스킬이 나머지를 처리합니다 — 개념 매핑, 레이아웃, JSON 생성, 렌더링, 시각적 유효성 검사.

## 결과물

스킬은 두 가지 파일을 함께 생성합니다:

- **`.excalidraw` 파일** — 편집 가능한 Excalidraw JSON 파일입니다. [excalidraw.com](https://excalidraw.com) 또는 Excalidraw 에디터에서 열어 도형, 텍스트, 색상, 배치 등을 직접 세부 수정할 수 있습니다.
- **PNG 파일** — `.excalidraw` 파일 옆에 함께 출력되는 렌더링 이미지입니다. 문서나 슬라이드에 바로 삽입할 수 있습니다.

다이어그램이 마음에 들지 않거나 일부만 바꾸고 싶다면 `.excalidraw` 파일을 직접 편집하거나, 에이전트에게 수정을 요청하세요. 에이전트는 수정 후 다시 PNG로 렌더링하여 결과를 확인합니다.

## 색상 커스터마이징

`references/color-palette.md`를 편집하여 브랜드에 맞게 설정합니다. 스킬의 나머지 부분은 모두 범용 디자인 방법론입니다.

## 파일 구조

```
gen-excalidraw-diagram-skill/
  SKILL.md                          # 디자인 방법론 + 워크플로우
  SKILL.ko.md                       # SKILL.md 한글 번역
  README.md                         # 설정 및 사용 가이드 (한글)
  README.en.md                      # README.md 영문 버전
  references/
    color-palette.md                # 브랜드 색상 (커스터마이징하려면 이 파일을 편집)
    element-templates.md            # 각 요소 타입별 JSON 템플릿
    json-schema.md                  # Excalidraw JSON 형식 참조
    render_excalidraw.py            # .excalidraw를 PNG로 렌더링
    render_template.html            # 렌더링용 브라우저 템플릿
    pyproject.toml                  # Python 의존성 (playwright)
```
