# True Time

True Time은 Google의 논문, Spanner에서 처음 소개된 고가용성 분산 시계입니다.

## Time Sync is HARD

분산 시스템에서 이벤트의 순서를 나타내는 것은 중요한 문제입니다. Lamport는 [인과적 순서라는 개념](./lamport-clock.md)을 만들어 부분적인 순서를 나타내는 방법을 고안합니다. 분산 시스템에서 시간을 맞추기 불가능하다는 것은 매우 유명한 사실입니다. 따라서 이 문서에서 시간을 맞추는게 어렵다는 설명을 생략합니다.

## Total Order Broadcast is Important

분산 시스템에서의 분산 합의 문제는 매우 중요합니다. 누가 리더인지 선택하거나, 자기 노드에 해당하는 hash range가 얼마나 되는지 등의 곳에서 분산 합의가 사용됩니다. 이러한 분산 합의 문제는 Total Order Broadcast(aka Atomic Broadcast)문제와 동일합니다. ref: [Unreliable Failure Detectors for Reliable
Distributed Systems](https://dl.acm.org/doi/pdf/10.1145/226643.226647)

## True Time API를 이용한 일관성 보장


