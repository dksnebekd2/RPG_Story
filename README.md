# RPG Story

<p align="center" width="100%">
<img src="highschool.png" alt="RPG_Story 맵" style="width: 80%; min-width: 300px; display: block; margin: auto;">
</p>

## 프로젝트 개요

정해진 스토리 안에서만 반복되는 MMORPG 서사의 한계를, AI가 큰 스토리의 틀을 벗어나지 않는 선에서 동적으로 스토리를 생성함으로써 개선할 수 있는지 검증하기 위한 시뮬레이션 프로젝트다. '마을 회장 선거'를 배경으로, 플레이어의 선택에 따라 NPC들의 여론이 어떻게 변화하는지를 실험했다.

> ※ 본 저장소는 개발 과정의 버전이며, 최종 실험에 사용한 버전과는 차이가 있다.

## 기반 프로젝트

본 프로젝트는 Stanford의 Generative Agents(Park et al., 2023) 오픈소스 코드베이스(reverie)를 기반으로 한다. Memory Stream 구조와 시뮬레이션 백엔드는 원 코드베이스를 그대로 활용했으며, 여기에 맵·조작·실험 시나리오를 직접 구현·수정하여 확장했다.

- 원 저작물: [Generative Agents: Interactive Simulacra of Human Behavior](https://github.com/joonspk-research/generative_agents) (Apache-2.0 License)

## 직접 구현 / 수정한 부분

### 맵 제작 및 수정
- 실험용 신규 타일맵(JSON)을 Tiled로 직접 제작하여 추가했다.
- 타일셋 참조 오류·오타를 수정해 맵 로딩 에러를 해결했다.
- Wall 레이어의 잘못된 타일셋 참조를 수정했다.

### 코드 버그 수정
- 날짜 파싱 포맷 오류를 수정했다 (`%B` → `%b`, meta.json 형식 불일치 해결).
- 시뮬레이션 결과 저장 시 폴더 미존재로 발생하던 오류를 방지하는 코드를 추가했다.

### OpenAI API 마이그레이션
- 2023년 기준으로 작성된 구형 OpenAI API 호출부를 최신 SDK 문법으로 전면 수정했다.
- 지원 종료된 구식 모델을 사용 가능한 모델로 일괄 교체·조정했다.

### 시뮬레이션 구성
- 실험용 시뮬레이션 저장소 및 페르소나(NPC) 데이터를 구성했다.
- 플레이어 조작 기능을 추가했다.
- '마을 회장 선거' 실험 시나리오를 설계하고 진행했다.

## 기술 스택

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Django](https://img.shields.io/badge/Django-092E20?style=for-the-badge&logo=django&logoColor=white)
![Phaser](https://img.shields.io/badge/Phaser-000000?style=for-the-badge&logo=phaser&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=for-the-badge&logo=openai&logoColor=white)

## 실험 내용

총 6명의 피실험자를 대상으로 크게 두 가지를 검증했다. 하나는 플레이어의 선택이 실제로 스토리 분기와 NPC 여론에 영향을 미치는지, 다른 하나는 이 시뮬레이션이 실사용 가능한 속도로 동작하는지다.

### 1. 플레이어 선택에 따른 NPC 여론 변화 검증

'마을 회장 선거'를 배경으로, 피실험자가 후보 NPC(Wolfgang Schulz)의 공약과 행동을 직접 선택하면 나머지 NPC들의 여론이 어떻게 갈리는지 관찰했다. 비교 대상 후보는 Abigail Chen으로 고정했다. 아래는 6명 중 상반된 결과를 보인 두 사례다.

**Player 1 — '토론 제의' 선택**
- 공약: 하수구 수리, 새벽 안내방송 폐지, 치안 강화 (현실적·단기적 문제 해결)
- Abigail 공약: 디지털 교육 시스템, 스마트 마을 프로젝트, 디지털 마켓플레이스 (미래 지향적)
- Player 1은 지지자를 늘리기 위해 '토론'을 제안했고, 이에 따라 NPC 22턴의 대화가 자동으로 오갔다.
- 결과는 기대와 반대였다: 토론 이후 Wolfgang 지지자는 3명으로 줄고, Abigail 지지자는 10명으로 늘었다. 현실적인 공약임에도 토론 과정에서 오히려 설득력을 잃은 것으로 나타났다.

**Player 3 — '연설' 선택**
- 공약: 장학재단 설립·운영, 장학금 제도, 주말 체험교실 운영 (교육 지원 중심)
- '연설'을 선택했으나 연설 직후에는 지지자 변동에 큰 영향이 없었다.
- 다만 최종적으로는 Wolfgang 지지자 8명, Abigail 지지자 5명으로, '장학재단'이라는 공약 자체가 NPC들에게 긍정적으로 받아들여지며 좋은 결과로 이어졌다.

**공통적으로 확인된 점**
- 같은 목표(당선)를 두고도 플레이어의 공약·선택에 따라 스토리의 과정과 결과가 실제로 달라졌다.
- NPC의 성격과 나이가 반응에 큰 영향을 미쳤다. 연령대가 높은 NPC는 안정적인 변화와 보장된 미래를 중시했고, 연령대가 낮은 NPC는 혁신적인 변화와 창의적인 비전에 더 매력을 느끼는 경향을 보였다.

**피실험자 설문 결과 (참여 인원 6명)**

| 문항 | 긍정 | 부정 |
|---|---|---|
| 1. 현재 MMORPG(메이플스토리, 로스트아크 등)의 스토리 시스템 평가 | 50% | 50% |
| 2. NPC들의 반응과 대화는 어떻게 느껴졌는가 | 83.3% | 16.7% |
| 3. 이번 실험의 'AI 기반 스토리 생성 기술'에 대한 생각 | 100% | - |
| 4. 실제 MMORPG 적용 시 다양한 스토리 경로 생성 가능성 | 83.3% | 16.7% |
| 5. 기존 MMORPG 스토리의 한계 개선에 도움이 될지 | 100% | - |

*(응답은 '매우 긍정적'~'매우 부정적' 6단계 척도로 수집한 뒤 긍정/부정 2개 범주로 집계한 것이다.)*

대부분의 피실험자가 AI 기반 스토리 생성 기술의 가능성을 긍정적으로 평가했으며, 일부는 개선이 필요하다고 응답했다.

### 2. 시뮬레이션 진행 속도 저하 문제 발견

당초 시뮬레이션은 48시간(step 8640) 연속 실행을 목표로 설계했다. 그러나 테스트 과정에서 스텝이 진행될수록 처리 속도가 급격히 느려지는 문제를 발견했다.

- 시뮬레이션 시간 기준 1 step은 10초이며, 목표한 24시간 분량은 step 8640에 해당한다.
- 실제로 step 4320(시뮬레이션 시간 12시간 분량)을 실행한 결과, 2024년 12월 1일 오후 12시 34분부터 12월 2일 오전 1시 36분까지 총 13시간 2분(46,920초)이 소요됐다.
- 이를 기반으로 계산한 스텝당 평균 처리 시간은 10.86초로, 시뮬레이션 내 1 step(10초)보다 이미 느린 속도였다.
- 전체 구간을 절반으로 나눠 비교하면(총 소요 시간을 초반 30% : 후반 70%로 가정한 근사치) 초반 구간은 약 6.52초/step, 후반 구간은 약 15.21초/step로, 시간이 지날수록 처리 속도가 뚜렷하게 저하되는 경향을 보였다.
- 이 추세를 그대로 적용하면 당초 목표였던 step 8640(24시간) 완주에는 약 26시간 4분이 소요될 것으로 예상되며, 이는 48시간 연속 실행이라는 원래 계획이 사실상 어렵다는 것을 의미한다.

이상적인 경우라면 스텝당 처리 시간이 일정 수준(약 7.6초/step)에서 수렴해야 하지만, 실제 테스트 결과는 수렴 없이 계속 우상향하는 추세를 보였다.

## Acknowledgements

Memory Stream 구조와 시뮬레이션 백엔드를 비롯해 이 프로젝트의 근간이 되어준 Generative Agents 원저자분들께 감사드립니다. 이 오픈소스가 없었다면 이 프로젝트는 시작할 수 없었을 것 입니다.

맵 타일, 가구, 캐릭터 스프라이트 등 비주얼 에셋 역시 Generative Agents 오픈소스에 포함된 리소스를 그대로 재사용 했습니다. 원작자분들께 감사드립니다.

- 배경 아트: [PixyMoon (@_PixyMoon_)](https://twitter.com/_PixyMoon_)
- 가구/인테리어 디자인: [LimeZu (@lime_px)](https://twitter.com/lime_px)
- 캐릭터 디자인: [ぴぽ (@pipohi)](https://twitter.com/pipohi)

```bibtex
@inproceedings{Park2023GenerativeAgents,
  author = {Park, Joon Sung and O'Brien, Joseph C. and Cai, Carrie J. and Morris, Meredith Ringel and Liang, Percy and Bernstein, Michael S.},
  title = {Generative Agents: Interactive Simulacra of Human Behavior},
  year = {2023},
  publisher = {Association for Computing Machinery},
  booktitle = {Proceedings of the 36th Annual ACM Symposium on User Interface Software and Technology (UIST '23)},
  series = {UIST '23}
}
```

## License

원본 [generative_agents](https://github.com/joonspk-research/generative_agents) 저장소를 따라 Apache-2.0 라이선스를 적용한다.
