# Phi Accrual Failure Detector
Phi Accrual Failure Detector는 Failure Detector의 한 종류입니다.

## Background
failure를 탐지하는 것은 매우 중요한 문제입니다. 노드에 장애가 발생했는지 확인해야하며, 이 방법에 따라서 가용성이나 일관성에 영향을 줄 수도 있습니다. 따라서 우리는 빠르고 정확하게 노드의 장애를 탐지해야합니다.

기존에는 단순한 heart beat을 호출하고, timeout을 넘으면 실패하는 방식을 사용합니다. 이 방식은 간단하며, 명확합니다.

하지만 장애(failure)를 탐지하는 것은 그렇게 쉽지않습니다. 네트워크는 기본적으로 느려질 수 있으며, 실패할 수 있습니다. 또한 네트워크뿐만아니라 노드에서 응답을 처리하는 도중에 GC가 실행되는 등의 이유로 응답이 늦어질 수 있습니다.

이런 응답 지연문제는 영원히 지속될 수도 있으며, heart beat의 timeout이 어떻게 설정되었는지에 따라서 살아있는 노드에 문제가 발생했다고 감지할 수도 있습니다. 

## Phi Accural Failure Detector
우리는 앞에서 단순한 timeout이 장애를 판단하는데 완벽한 방법이 아니라는 것을 살펴보았습니다. 그렇다면 어떤 방법이 있을까요? 많은 시스템에서 Phi Accural Failure Detector를 사용하여 장애를 보다 나은 방법으로 감지하고 있습니다.

Phi Accural Failure Detector(PAFC)는 장애가 발생한 확률을 구하는 방식으로 이 문제를 보완합니다. 매번 heart beat를 보내고, 평균적으로 발생하는 네트워크 지연을 계산하여 장애가 난 상황을 감지합니다.

네트워크 환경에서 일어날 수 있는 변동을 고려할 수 있기에, 이런 방식은 꽤나 바람직한 접근입니다. 또한 단순히 이분법으로 장애를 판단하는 것이 아닌, 샘플링한 하트비트에 따라서 장애 가능성을 유동적으로 조정할 수 있습니다.

akka에서는 장애로 판단하는 phi의 값을 8로 설정하며, 불안정한 네트워크 상황에서는 12로 설정한다고합니다.

## Detail
phi의 값은 보통 다음과 같이 계산됩니다.
```
phi = -log10(1 - F(마지막에 도착한 하트비트에 걸린 시간))
```

여기에서 F함수는 하트비트에 대한 네트워크 지연으로 추정된 평균, 표준편차가 있는 정규 분포의 누적 분포 함수입니다. 자세한 구현은 아래 akka의 구현을 참고하시길바랍니다.

```scala
/**
* Calculation of phi, derived from the Cumulative distribution function for
* N(mean, stdDeviation) normal distribution, given by
* 1.0 / (1.0 + math.exp(-y * (1.5976 + 0.070566 * y * y)))
* where y = (x - mean) / standard_deviation
* This is an approximation defined in β Mathematics Handbook (Logistic approximation).
* Error is 0.00014 at +- 3.16
* The calculated value is equivalent to -log10(1 - CDF(y))
*/

private[akka] def phi(timeDiff: Long, mean: Double, stdDeviation: Double): Double = {
    val y = (timeDiff - mean) / stdDeviation
    val e = math.exp(-y * (1.5976 + 0.070566 * y * y))
    if (timeDiff > mean)
        -math.log10(e / (1.0 + e))
    else
        -math.log10(1.0 - 1.0 / (1.0 + e))
}
```
## Other Failure Detector
장애 탐지기는 Phi Accrual Detector을 제외하고도 여러가지 개선된 친구들이 많이 있습니다. 한번 살펴보시면 재밌을 것 같아요!!

### See Also
  - https://doc.akka.io/docs/akka/current/typed/failure-detector.html
  - https://github.com/akka/akka/blob/main/akka-remote/src/main/scala/akka/remote/PhiAccrualFailureDetector.scala
  - http://www.cs.cornell.edu/projects/ladis2009/papers/lakshman-ladis2009.pdf
  - https://dl.acm.org/doi/10.1145/1244002.1244129
