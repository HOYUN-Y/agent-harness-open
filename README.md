# Agent Harness Open

Claude Code와 Codex에서 사용하는 공개 스킬 7개와 커스텀 에이전트 2개입니다. 이 저장소에 포함된 항목만 배포합니다.

## 스킬

| 스킬 | 용도 |
|---|---|
| [inspecting-vibe](skills/inspecting-vibe/SKILL.md) | 웹·그래픽·영상 결과물의 디자인 품질 검수 |
| [motion-web-builder](skills/motion-web-builder/SKILL.md) | 모션 중심 웹 페이지 설계·구현·검증 |
| [herdr-workflow](skills/herdr-workflow/SKILL.md) | Herdr 터미널·에이전트 협업과 초기 설정 |
| [changelog-update](skills/changelog-update/SKILL.md) | 변경 파일을 바탕으로 작업 기록 갱신 |
| [changelog-verify](skills/changelog-verify/SKILL.md) | 작업 기록의 누락 검증·보강 |
| [ai-use-case-research](skills/ai-use-case-research/SKILL.md) | 소셜 플랫폼의 AI 활용 사례 조사 |
| [secondbrain-index-sync](skills/secondbrain-index-sync/README.md) | 세컨드브레인 색인·자료 연결 관리 |

## 설치와 갱신

```sh
git clone https://github.com/HOYUN-Y/agent-harness-open.git
cd agent-harness-open
```

원하는 `skills/<이름>/` 폴더 전체를 복사합니다. Codex의 설치 위치는 `${CODEX_HOME:-$HOME/.codex}/skills`, Claude Code는 `~/.claude/skills`입니다. 기존 `~/.agents/skills` 설치도 확인해 같은 이름을 중복 설치하지 마세요. 호출은 Codex의 `$스킬이름`, Claude Code의 `/스킬이름`입니다. 기존 설치본을 자동으로 덮어쓰지 말고 개인 설정과 내용을 먼저 비교하세요.

개발 원본을 사용하는 macOS·Linux 환경에서는 폴더를 심볼릭 링크로 연결할 수도 있습니다. 링크 설치는 커밋 전 수정과 브랜치 전환도 즉시 반영합니다. 안정적인 버전이 필요하면 폴더를 복사하고 새 세션에서 확인하세요. Windows에서는 폴더 전체 복사를 사용할 수 있습니다.

- **changelog:** 두 스킬을 나란히 설치합니다. `python3 install-changelog.py --skills-dir "/path/to/skills" --target-root "/path/to/work"`로 복사 설치할 수 있습니다. 작업 경로·스탬프·운영 설정은 공개 저장소에 넣지 마세요. 설치만으로 스케줄러가 등록되지는 않습니다.
- **secondbrain:** [스킬별 안내](skills/secondbrain-index-sync/README.md)를 따릅니다. 설치 스크립트는 기존 설치 위치와 볼트 설정에 영향을 줄 수 있으므로 먼저 검토하세요. 개인 자료나 볼트 자체는 이 저장소에 포함하지 않습니다.
- **herdr:** [초기 설정 안내](skills/herdr-workflow/references/setup.md)를 따릅니다. 스킬 복사만으로 전역 지침이나 상태 훅이 설치되지는 않습니다.
- **motion:** 스킬에 적힌 캡처 도구의 런타임 요구를 확인하세요. 엔진의 [상류 출처](skills/motion-web-builder/engine/UPSTREAM.md)와 [MIT 라이선스](skills/motion-web-builder/engine/LICENSE)를 보존합니다.
- **research:** 플랫폼 탐색을 위한 브라우저 도구가 필요합니다. 접근 권한과 로그인 상태에 따라 조사 범위가 달라집니다.

## 커스텀 에이전트

| 에이전트 | 역할 |
|---|---|
| [code-reviewer](reviewer/README.md) | 변경사항·전체 코드·특정 영역 리뷰. 확인된 결함과 개선 제안 분리 |
| [advisor](advisor/README.md) | 계획·반복 오류의 원인 분석·완료 판단 자문. Astra·Fable 지정 |

스킬 설치와 별개로 프로젝트에 정의 파일을 생성합니다. Node.js 20 이상이 필요하며 새 의존성은 없습니다.

```sh
node reviewer/scripts/build-agents.mjs --project "/path/to/project"
node reviewer/scripts/build-agents.mjs --project "/path/to/project" --check
node reviewer/scripts/build-agents.mjs --project "/path/to/project" --agent advisor
node reviewer/scripts/build-agents.mjs --project "/path/to/project" --agent advisor --check
```

생성 위치는 프로젝트의 `.claude/agents/`와 `.codex/agents/`입니다. 기존 파일과 다르면 중단하고 심볼릭 링크를 따라 설치하지 않습니다. 모델·권한의 실제 적용 여부와 검증 한계는 각 에이전트 안내를 확인하세요. advisor의 실제 모델 호출은 아직 검증하지 않았습니다.

## 로컬 검증

Python·Node.js 표준 모듈을 사용합니다. TOML 검사는 Python 3.11 이상이 필요합니다.

```sh
python3 tests/check_portability.py
node reviewer/tests/build-agents.mjs
python3 -I reviewer/tests/fixtures.py
bash -n skills/changelog-update/scripts/changed-files.sh
bash -n skills/changelog-update/scripts/cron-run.sh
bash -n skills/changelog-verify/scripts/check-detail.sh
node skills/motion-web-builder/tests/video-hero.mjs
python3 -I skills/secondbrain-index-sync/scripts/check_index.py --selftest
python3 -I skills/secondbrain-index-sync/scripts/inventory.py --selftest
python3 -I skills/secondbrain-index-sync/scripts/manifest.py --selftest
python3 -I skills/secondbrain-index-sync/scripts/doc_manifest.py --selftest
python3 -I skills/secondbrain-index-sync/scripts/build_topics.py --selftest
git diff --check
```

자동 검사는 실제 웹 화면·영상 재생·에이전트 실행 품질을 대신하지 않습니다. GitHub Actions에는 스킬 검사와 세컨드브레인 운영체제별 검사를 포함합니다.

## 출처와 배포 범위

이전에 가져온 스킬별 원본 커밋은 [출처 목록](docs/import-sources.json)에 있습니다. 공개본 배포 기록은 [WORKLOG](WORKLOG.md)에 남깁니다. 제3자 저작권·라이선스·출처를 유지하며 저장소 전체에 새 라이선스를 일괄 적용하지 않습니다. MIT 라이선스가 있는 엔진의 조건이 모든 스킬에 적용되는 것은 아닙니다.
