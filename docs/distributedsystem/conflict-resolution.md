# Conflict Resolution

데이터가 여러 노드에 분산되어 저장되는 경우, 데이터는 일치하지 않을 수 있습니다.

동시에 수정하게 된다면 우리는 두개의 변경사항을 최대한 말이 되게 적용할 필요가 있습니다.
이 경우에 어떻게 처리해야하는지 방법들을 기술하며, 각 데이터베이스가 어떤 선택을 했는지 요약합니다.

## LAST WRITE WIN (aka. LWW)

마지막에 들어온 요청을 사용하는 방식입니다. 클라이언트에서 요청을 보내는 순간 

하지만 요청이 취소될 가능성이 있기 때문에 지속성(Durablity)가 떨어진다고 볼 수 있습니다.


## See Also

- https://martin.kleppmann.com/2016/11/03/code-mesh.html
