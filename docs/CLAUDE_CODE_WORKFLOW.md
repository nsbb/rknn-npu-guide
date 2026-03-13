# Claude Code 작업 워크플로우

RK3588 NPU 포팅 작업에서 Claude Code를 효율적으로 사용하기 위한 규칙.

---

## 1. 세션 시작 위치 — 항상 상위 폴더

```bash
cd /home/rk3588/travail/rk3588
claude
```

**이유:**
- Claude Code의 memory 경로가 작업 디렉토리 기반으로 결정됨
- 상위 폴더 고정 → memory가 `~/.claude/projects/-home-rk3588-travail-rk3588/memory/` 한 곳에 축적
- 프로젝트별로 열면 memory가 분산되어 이전 작업 경험을 못 가져옴
- 한 모델에서 발견한 RKNN 버그/우회법이 다음 모델 포팅 시 자동으로 적용됨

**하지 않는 것:**
```bash
# ✗ 이렇게 하면 memory가 분산됨
cd /home/rk3588/travail/rk3588/rknn-stt && claude
cd /home/rk3588/travail/rk3588/rknn-wakeword && claude
```

---

## 2. CLAUDE.md 계층 구조

```
rk3588/
├── CLAUDE.md                    ← 항상 로드 (전체 구조, 필독 순서)
├── docs/RK3588_NPU_AI_GUIDE.md  ← 범용 가이드 (CLAUDE.md에서 참조)
├── rknn-wakeword/
│   └── CLAUDE.md                ← 해당 폴더 접근 시 추가 로드
└── rknn-stt/
    └── CLAUDE.md                ← 해당 폴더 접근 시 추가 로드
```

- 상위 CLAUDE.md: 모든 세션의 공통 컨텍스트 (필독 순서, 프로젝트 목록)
- 하위 CLAUDE.md: 특정 프로젝트 작업 시에만 참조
- Claude는 상위에서 시작해도 하위 CLAUDE.md를 파일 접근 시 자동 로드

---

## 3. Memory 관리

### 위치
```
~/.claude/projects/-home-rk3588-travail-rk3588/memory/
├── MEMORY.md              ← 인덱스 (200줄 이내, 매 세션 로드)
├── feedback_*.md          ← 사용자 피드백
├── project_*.md           ← 프로젝트 상태
└── reference_*.md         ← 외부 참조
```

### 규칙
- MEMORY.md 인덱스는 200줄 이내 유지 (초과분은 로드되지 않음)
- 상세 내용은 개별 topic 파일에 저장 (필요할 때만 읽음 → 컨텍스트 절약)
- 프로젝트 완료 시 status 업데이트, 오래된 memory 정리
- 코드에서 파악 가능한 정보는 저장하지 않음

---

## 4. 세션 분리 기준

세션을 나누는 시점:
- 작업 단위가 완전히 끊길 때 (zipformer 완료 → wav2vec2 시작)
- 컨텍스트가 압축되기 시작할 때 (긴 디버깅 세션 후)
- 다음 날 새로 시작할 때

세션을 유지하는 경우:
- 같은 모델의 연속 작업 (변환 → 디버깅 → 테스트)
- 짧은 작업 여러 개

**핵심:** 세션은 자유롭게, 위치만 고정.

---

## 5. 새 모델 추가 시 체크리스트

```bash
# 1. 상위 폴더에서 세션 시작
cd /home/rk3588/travail/rk3588 && claude

# 2. 첫 메시지
# "CLAUDE.md 읽고 지시대로 문서 다 읽은 다음 rknn-xxx 포팅 시작해줘"

# 3. Claude가 읽는 순서 (CLAUDE.md에 명시됨)
#   ① docs/RK3588_NPU_AI_GUIDE.md
#   ② rknn-wakeword/docs/RKNN_PORTING_GUIDE.md
#   ③ 대상 프로젝트 CLAUDE.md

# 4. 포팅 작업 → 결과를 가이드에 반영

# 5. 새 버그 발견 시
#   → docs/RK3588_NPU_AI_GUIDE.md 업데이트
#   → 다음 모델 포팅 시 자동 적용
```

---

## 6. Git 레포 구조

```
rk3588/                          ← git 레포 아님, 작업 공간
├── rknn-wakeword/               ← git 레포 (nsbb/rknn-wakeword)
├── rknn-stt/                    ← git 레포 (nsbb/rknn-stt)
│   ├── zipformer/
│   └── wav2vec2/
├── rknn-tts/                    ← 미래
└── docs/                        ← 범용 가이드 (git 관리 X)
```

- 모델 카테고리별로 레포 분리 (STT 모델들은 하나의 레포에 묶음)
- 각 레포의 CLAUDE.md는 레포에 커밋
- 상위 CLAUDE.md와 docs/는 로컬 전용
