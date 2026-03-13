# rknn-npu-guide

RK3588 NPU에서 AI 모델(웨이크워드, STT, TTS, LLM)을 포팅할 때 쓰는 범용 가이드.

직접 겪은 RKNN-Toolkit2 버그, 우회법, 디버깅 순서를 정리했다. 새 모델 포팅 전에 먼저 읽으면 같은 실수를 반복하지 않는다.

## 문서

| 파일 | 내용 |
|------|------|
| [docs/RK3588_NPU_AI_GUIDE.md](docs/RK3588_NPU_AI_GUIDE.md) | RKNN 버그 목록, 우회법, 태스크별 주의사항, 디버깅 체크리스트 |
| [docs/CLAUDE_CODE_WORKFLOW.md](docs/CLAUDE_CODE_WORKFLOW.md) | Claude Code 세션/memory 관리 워크플로우 |

## 프로젝트 레포

| 레포 | 태스크 | 상태 |
|------|--------|------|
| [nsbb/rknn-wakeword](https://github.com/nsbb/rknn-wakeword) | 웨이크워드 (BCResNet-t2) | 완료 — 98.68% acc, FAR 0.00/hr |
| [nsbb/rknn-stt](https://github.com/nsbb/rknn-stt) | 한국어 STT (Zipformer, wav2vec2) | 진행 예정 |

## 환경

```
하드웨어:  RK3588 (Cortex-A76×4 + A55×4, Mali-G610, NPU 6TOPS)
OS:        Linux 5.10 (Rockchip BSP)
Python:    3.8 (conda env: RKNN-Toolkit2)
Toolkit:   RKNN-Toolkit2 v2.3.2 / rknn-toolkit-lite2 v2.3.2
```
