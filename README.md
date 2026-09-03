# RPG Story

<p align="center" width="100%">
<img src="highschool.png" alt="RPG_Story 맵" style="width: 80%; min-width: 300px; display: block; margin: auto;">
</p>

---

정해진 스토리 안에서만 반복되는 MMORPG 서사의 한계를, AI가 큰 틀을 벗어나지 않는 선에서 스토리를 동적으로 생성함으로써 개선할 수 있는지 검증한 시뮬레이션이다. '마을 회장 선거'를 배경으로, 플레이어의 선택에 따라 13명 NPC의 여론이 어떻게 갈리는지를 6명의 피실험자를 대상으로 실험했다.

## 기술 스택

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Django](https://img.shields.io/badge/Django-092E20?style=for-the-badge&logo=django&logoColor=white)
![Phaser](https://img.shields.io/badge/Phaser-000000?style=for-the-badge&logo=phaser&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=for-the-badge&logo=openai&logoColor=white)

## 기반 프로젝트

Stanford의 Generative Agents(Park et al., 2023) 오픈소스 코드베이스(reverie)를 기반으로 한다. Memory Stream 구조와 시뮬레이션 백엔드, 비주얼 에셋은 원 코드베이스의 것을 활용했으며, 여기에 맵·플레이어 조작·실험 시나리오를 직접 구현·수정해 확장했다.

- 원 저작물: [Generative Agents: Interactive Simulacra of Human Behavior](https://github.com/joonspk-research/generative_agents) (Apache-2.0 License)

## 직접 구현 / 수정한 부분

- **신규 맵 제작** — Tiled로 140×80 타일 규모의 HighSchool 월드를 설계. 섹터 4개, 아레나 26개, 상호작용 오브젝트 25종, 스폰 포인트 15개를 `World:Sector:Arena:Object` 주소 체계로 정의하고 CSV 스키마로 분리
- **Phaser 연동 재작성** — 타일셋 매핑, 레이어 생성 순서와 depth 정렬, 충돌 레이어의 물리 시스템 연결
- **플레이어 조작 구현** — 원본의 방향키 입력은 충돌 판정 없이 카메라만 움직이는 앵커였다. WASD 입력으로 바꾸고 충돌 레이어와 연결해 캐릭터가 벽·가구에 실제로 막히도록 처리 *(최종 버전에서 구현, 본 저장소에는 미반영)*
- **OpenAI API 마이그레이션** — 2023년 기준으로 작성돼 실행되지 않던 호출부를 신규 SDK 문법으로 전면 수정하고, 지원 종료된 모델을 일괄 교체
- **버그 수정** — 날짜 파싱 포맷 불일치(`%B` → `%b`), 결과 저장 시 폴더 미존재로 발생하던 크래시, 타일셋 참조 오류
- **실험 설계** — NPC 페르소나 데이터(성격·배경·목표·생활 패턴·공간 기억) 작성 및 '회장 선거' 시나리오 구성

## 저장소 상태

본 저장소는 개발 중간 버전이다. 최종 실험에 사용한 버전은 발표 이후 푸시하지 못했고 로컬 원본도 유실됐다. 최종 실험에 사용한 맵 에셋이 포함돼 있지 않아 **현재 코드는 그대로 실행되지 않으며, 개발 이력 확인용입니다.**

직접 제작한 HighSchool 맵 역시 최종 실험에는 사용하지 못했습니다. 백엔드용 매트릭스 데이터를 내보내는 과정에서 값이 어긋나 AI 시뮬레이션 단계에서 오류가 발생했고, 일정상 원본 맵으로 전환했습니다.

## Acknowledgements

Memory Stream 구조와 시뮬레이션 백엔드를 비롯해 이 프로젝트의 근간이 되어준 Generative Agents 원저자분들께 감사드립니다. 이 오픈소스가 없었다면 이 프로젝트는 시작할 수 없었을 것입니다.

맵 타일, 가구, 캐릭터 스프라이트 등 비주얼 에셋 역시 Generative Agents 오픈소스에 포함된 리소스를 재사용했습니다. 원작자분들께 감사드립니다.

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
