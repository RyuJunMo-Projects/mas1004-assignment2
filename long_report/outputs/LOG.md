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

## 2026-11-09 smooth03 (Track A, seed 0)
- changed: `label_smoothing` in `loss_fn` of `track_a/recipe.py`, from 0.1 to
  0.3. Nothing else differs from `baseline`: all training photos, the same
  augmentation, the same optimiser and schedule, seed 0 only.
- why: 라벨 스무딩이 되어있었음에도 자신있게 틀린 오답이 있었으니 라벨 스무딩 값을 바꾼다. 개선 기준은 0.01이다.
- validation: 0.4932 (before: 0.503 with seed 0, 0.494 with seed 1). Per city:
  seoul 0.577, busan 0.362, tokyo 0.650, osaka 0.340, taipei 0.537. 38.0 minutes
  on a Colab T4, 249,984 photos.
- what it means: 이전에 나온 시드간 변동을 반올림해 정해둔 기준인 0.01 이상 결과가 바뀌지 않았으니 개선되었다고 말하기 어렵다.

## 2026-11-09 crop07 (Track A, seed 0)
- changed: `scale` in `T.RandomResizedCrop(224, ...)` of `train_transform` in
  `track_a/recipe.py`, from (0.35, 1.0) to (0.7, 1.0). Nothing else differs from
  `smooth03`: the same flip, the same colour jitter, label smoothing still 0.3,
  all training photos, seed 0 only.
- why: 구조물이나 사물이 너무 메인이여서 생기는 문제를 방지하기 위해 크롭을 약하게 한다 (크롭 하한 0.35 → 0.7). 기준선 대비 0.01 이상 오르지 않으면 개선되었다고 말하기 어렵다.
- validation: 0.4927 (before: 0.4932 with smooth03 seed 0; 0.503 with baseline
  seed 0 and 0.494 with baseline seed 1). Per city: seoul 0.5733, busan 0.377,
  tokyo 0.623, osaka 0.3675, taipei 0.5225. 10887.5 seconds (181.5 minutes) on
  the CPU, 249,984 photos.
- what it means: 이번에도 직전에 라벨 스무딩을 조정한 모델보다 검증점수 차이가 -0.0005 밖에 나지 않았다. 앞서 정한 기준 0.01보다 작은 숫자이기 때문에 모델이 개선되었다고 보기 어렵다.

## 2026-11-09 b1 (Track B)
- changed: nothing — `src/train_b.py` as it came: ResNet18 at 224 px from the
  IMAGENET1K_V1 weights, 4 epochs over all of `data/starter/train`, no extra
  photos, on a Colab T4. 176,896 photos through the model, 1,565.9 s.
- why: Track B에서 나중에 설정을 바꿨을 때 좋아졌는지 비교할 기준이 필요했다. 그래서 처음에는 `train_b.py`의 기본 설정인 ResNet18, 224px, 4에폭으로 starter 학습 사진만 사용해 돌렸다.
- validation: 0.484 (before: nothing to compare with; this is the first Track B
  run). Per city: seoul 0.552, busan 0.382, tokyo 0.609, osaka 0.377, taipei
  0.499. 26.1 minutes on a Colab T4, 176,896 photos.
- what it means: 전체 검증 점수는 0.484였는데, 도시마다 차이가 컸다. 도쿄는 0.609였지만 부산은 0.382, 오사카는 0.377이었다. 다음 실험에서는 전체 점수가 오르는지와 함께, 특히 부산과 오사카를 더 잘 맞히는지도 보고 싶다.

## Plan for what to try next

다음에는 같은 모델과 학습 사진으로 에폭을 4에서 8로 늘려보고 싶다. 4에폭까지 학습 손실은 계속 줄었지만 검증 점수는 마지막에 한 번만 확인했다. 더 오래 학습했을 때 검증 점수도 오르는지, 그리고 내가 관심 있는 사물 중심 사진에서 오답이 줄어드는지 확인하고 싶다.
