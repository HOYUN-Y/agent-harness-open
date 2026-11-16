# Agent Harness Open

Claude Code·Codex용 스킬 7개와 커스텀 에이전트 2개입니다.

## 스킬

| 스킬 | 용도 |
|---|---|
| [inspecting-vibe](skills/inspecting-vibe/SKILL.md) | 웹·그래픽·영상 디자인 검수 |
| [motion-web-builder](skills/motion-web-builder/SKILL.md) | 모션 중심 웹 제작 |
| [herdr-workflow](skills/herdr-workflow/SKILL.md) | Herdr 터미널·에이전트 협업 |
| [changelog-update](skills/changelog-update/SKILL.md) | 작업 기록 갱신 |
| [changelog-verify](skills/changelog-verify/SKILL.md) | 작업 기록 검증·보강 |
| [ai-use-case-research](skills/ai-use-case-research/SKILL.md) | AI 활용 사례 조사 |
| [secondbrain-index-sync](skills/secondbrain-index-sync/README.md) | 세컨드브레인 색인 관리 |

## 설치

```sh
git clone https://github.com/HOYUN-Y/agent-harness-open.git
cd agent-harness-open
```

원하는 `skills/<이름>/` 폴더 전체를 Codex의 `${CODEX_HOME:-$HOME/.codex}/skills` 또는 Claude Code의 `~/.claude/skills`에 복사합니다. 호출은 Codex `$스킬이름`, Claude Code `/스킬이름`입니다. 추가 설정·실행 환경은 각 스킬 안내를 확인하세요.

## 커스텀 에이전트

- [code-reviewer](reviewer/README.md): 변경사항·전체 코드·특정 영역 리뷰.
- [advisor](advisor/README.md): 계획·원인 분석·완료 판단 자문. Astra·Fable 사용.

Node.js 20 이상에서 프로젝트용 정의를 생성합니다.

```sh
node reviewer/scripts/build-agents.mjs --project "/path/to/project"
node reviewer/scripts/build-agents.mjs --project "/path/to/project" --agent advisor
```

생성 위치는 `.claude/agents/`, `.codex/agents/`입니다. 기존 내용과 다르면 덮어쓰지 않습니다. advisor의 실제 모델 호출은 미검증입니다.

## 검증·출처

자동 검사는 [GitHub Actions](https://github.com/HOYUN-Y/agent-harness-open/actions), 상세 검증은 각 스킬·에이전트 안내를 확인하세요. TOML 검사에는 Python 3.11 이상이 필요합니다.

[원본 출처](docs/import-sources.json) · [작업 기록](WORKLOG.md) · [모션 엔진 MIT 라이선스](skills/motion-web-builder/engine/LICENSE)

제3자 라이선스·출처를 보존하며 저장소 전체에 새 라이선스를 일괄 적용하지 않습니다.
