# Apache Beam
Apache Beam은 스트리밍 처리 파이프라인을 정의하기 위한 통합 모델입니다. Beam으로 작성된 파이프라인은 여러가지 처리 엔진으로 이식될 수 있으며, Flink, Spark등의 Runner를 지원합니다.

## Basis
**Pipeline**
파이프라인은 Beam 모델의 가장 기본이 되는 개념입니다. 파이프라인은 데이터 처리 작업에 관련된 모든 DAG를 포함하고 있습니다. 데이터 Extract, Transform, Load를 수행하는 노드들도 포함됩니다. 그래프로 표현될 수 있는 거의 모든 작업이 Beam 파이프라인으로 표현될 수 있습니다.

이러한 파이프라인은 Beam SDK에서 작성되며, Runner로 옮겨지며 실행됩니다.

**PCollection**

PC는 정렬되지 않은 값들을 담고있는 콜렉션입니다. PC는 분산되어있을 가능성이 있으며, 데이터 셋이거나 데이터 스트림일 수 있습니다. PC들은 각각 특정 Pipeline에 속해있습니다. 하나의 PC를 여러 Pipeline이 동시에 소유하는 것은 불가능합니다. 파이프라인은 PCollection을 처리하고, Runner들은 이를 저장할 수 있습니다.



