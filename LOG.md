# Log

One entry for each recipe, selection or Track B model you train, newest at
the bottom. The numbers come from `experiments.csv`. "Why" and "what it means"
are in your own words: your agent asks you for them and does not make them up.
End the log with your plan for what to try next.

<!-- The form of an entry:

## <date> <tag> (Track A or B)
- changed: what is different from the run before
- why: your reason
- validation: 0.000 (before: 0.000)
- what it means: what you think the result says
-->

## 2026-11-09 baseline (Track A, seeds 0 and 1)
- changed: nothing. The starter recipe as it came with the assignment, all
  training photos (no `selection.csv`), trained twice on Colab, once with seed
  0 and once with seed 1. Only the seed differs between the two runs.
- why: 같은 설정에서도 무작위성에 의한 점수 변화가 얼마나 되는지 확인하려고
  두 번 돌렸다.
  (I ran it twice to see how much the score moves because of randomness alone,
  with the settings unchanged.)
- validation: 0.503 with seed 0, 0.494 with seed 1 (before: nothing to compare
  with; this is the first run). Per city, seed 0 / seed 1: seoul 0.587 / 0.573,
  busan 0.378 / 0.381, tokyo 0.639 / 0.612, osaka 0.346 / 0.380, taipei
  0.564 / 0.525. 37.6 and 37.1 minutes on a Colab T4, 249,984 photos each.
- what it means: 운에 의해 변동된 점수가 그 정도 차이이니 앞으로 레시피를
  바꿔서 테스트해 봤을 때 점수 변동이 그 이하면 함부로 개선되었다고 보기
  어렵다는 뜻이다.
  (That is how much the score moves by luck, so from now on, when I test a
  changed recipe, a change of less than that is not enough to call it an
  improvement.)

### Mistakes of `baseline` seed 0 (Step 3)

The 48 validation photos the model was most confident about and still got
wrong, one sheet per city. What they have in common, in the student's words:

공통점 1. 도시의 전체적인 모습보다 사물, 구조물에 더 포커스가 맞춰진 사진이다.
공통점 2. 시장, 지하철역, 공원 같이 세계 어디서나 볼 수 있는 장소들이다.
