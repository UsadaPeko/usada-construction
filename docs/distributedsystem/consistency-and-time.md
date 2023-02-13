# Consistency with Times

## What is consistency?

일관성은 컴퓨터 사이언스에서 다양하게 사용됩니다. 그리고 그 밖에서도 아주 많이 쓰이는 용어이죠. 우리는 이 글에서 좀 더 일관성에 대하여 잘 알아보는 시간을 갖고싶습니다. 일단 일관성을 정의해보아야겠죠. 일관성을 엄밀하게 정의하기 위해서는 어떤 영역에서 부르는지 같이 고려해야합니다.

### Consistency in Transaction Context

트랜잭션이라는 개념은 개발자에게 매우 익숙합니다. 특히 데이터베이스를 공부해보았다면 트랜잭션에 3가지 속성에 대하여 알고있을 것입니다. 우리는 트랜잭션 컨셉을 정리한 Jim Gery의 [The Transaction Concept: Virtues and Limitations](http://jimgray.azurewebsites.net/papers/thetransactionconcept.pdf)의 정의를 인용하면서 일관성을 정의해보겠습니다.

> ABSTRACT: A transaction is a transformation of state which has the properties of atomicity (all or nothing), durability (effects survive failures) and consistency (a correct transformation).
>
> 초록: 트랜잭션(transaction)은 원자성(전부 혹은 아무것도), 내구성(실패에서 살아남는 효과), 일관성(올바른 변환)의 특성을 갖는 상태의 변환이다.

초록을 살펴보면 일관성을 올바른 변환(a correct transformation)으로 정의하는 것을 살펴볼 수 있습니다. 올바른 변환이란  무엇일까요? 조금 더 논문을 살펴보겠습니다.

> The transaction concept derives from contract law. In making a contract, two or more parties negotiate for a while and then make a deal. The deal is made binding by the joint signature of a document or by some other act (as simple as a handshake or nod). If the parties are rather suspicious of one another or just want to be safe, they appoint an intermediary (usually called an escrow officer to coordinate the commitment of the transaction.
>
> 트랜잭션은 계약(contact) 조건에서 비롯된다. 계약을 진행할 때, 2 혹은 그 이상의 당사자가 얼마동안 협상을 하고 거래를 진행한다(make a deal). 거래(deal)는 여러명의 계약서 서명(joint signature of a document) 또는 악수 또는 고개 끄덕임처럼 단순한 다른 행위에 의해 구속력을 갖는다. (생략)

여기서 알 수 있는 내용은 트랜잭션이 여러가지 거래를 - 혹은 계약 - 잘 모델링하기 위하여 만들어졌다는 것입니다. 사실 이 내용은 뒤에 예시를 보면 더 잘 알 수 있는데, 여기서는 스킵합니다.

> The transaction concept emerges with the following properties:
> 
> - Consistency: the transaction must obey legal protocols.
> - Atomicity: it either happens or it does not; either all are bound by the contract or none are.
> - Durability: once a transaction is committed, it cannot be abrogated.
>
> 트랜잭션 개념은 다음의 속성들로 나타낼 수 있다.
> 
> - 일관성(Consistency): 트랜잭션은 적법한 프로토콜을 따라야 한다.
> - 원자성(Atomicity): 발생하거나 발생하지 않아야 한다. 계약에 모든 것이 구속되거나, 전혀 그렇지 않거나 해야 한다.
> - 지속성(Durability): 일단 트랜잭션이 확정되면(committed), 취소할 수 없다.

위와 같이 트랜잭션은 3가지 속성으로 나타내고 있으며, 논문의 뒷부분에서는 이 3가지 속성이 어떻게 실제 세계의 거래를 프로그래밍하는데 도와줄 수 있는지 살펴보고 있습니다. 우리는 여기에서 일관성에 집중해보겠습니다. 적법한 프로토콜이란 무엇일까요? 우리가 일반적으로 알고있는 일관성의 정의와는 사뭇 다릅니다.

*여담으로 ACID는 [Principles of Transaction-Oriented Database Recovery](https://web.archive.org/web/20190923153438/https://sites.fas.harvard.edu/~cs265/papers/haerder-1983.pdf)에서 처음 소개되며, 트랜잭션의 4가지 속성으로 늘어나게 됩니다.*

> Translating the transaction concept to the realm of computer science, we observe that most of the transactions we see around us (banking, car rental, or buying groceries) may be reflected in a computer as transformations of a system state.
> 
> A system state consists of records and devices with changeable values. The system state includes assertions about the values of records and about the allowed transformations of the values. These assertions are called the system consistency constraints.
> 
> The system provides actions which read and transform the values of records and devices. A
collection of actions which comprise a consistent transformation of the state may be grouped to
form a transaction. Transactions preserve the system consistency constraints -- they obey the
laws by transforming consistent states into new consistent states.
>
> 트랜잭션 개념을 컴퓨터 과학의 영역으로 이야기하면(translating), 우리 주변에서 볼 수 있는 대부분의 거래-transaction-(은행, 자동차 대여 또는 식료품 구입)가 시스템 상태의 변환(transformations)으로 컴퓨터에 반영될 수 있음을 관찰할 수 있다.
> 
> 시스템 상태는 변경 가능한 값을 가진 레코드와 장치로 구성됩니다. 시스템 상태에는 레코드의 값과 허용되는 값의 변환(transformations)에 대한 assertions이 포함됩니다. 이러한 assertions을 시스템 일관성 제약 조건이라고 합니다.
>
> 시스템은 레코드와 장치의 값을 읽고 변환하는 작업을 제공합니다. 상태의 일관된 변환을 구성하는 동작들의 집합은 트랜잭션을 형성하기 위해 그룹화될 수 있다. 트랜잭션은 시스템 일관성 제약 조건을 보존하며 -- 일관된 상태를 새로운 일관된 상태로 변환함으로써 법을 준수합니다.

위 문장에 따르면 시스템에서는 어떠한 불변식(assertions)이 있고, 그것을 만족하면서 상태를 변환해야합니다. 저는 이정도면 일관성에 대하여 충분히 transaction 개념의 입장을 들어본 것 같습니다. 이제 다른 분야에서 살펴보겠습니다.

### Consistency in Distributed System

분산 시스템에서의 Consistency는 여러가지 관점에서 사용됩니다. 우리는 CAP 정리에서부터 시작해보겠습니다.

CAP 정리는 분산 시스템은 Consistency, Availablity, Partition-tolerance를 동시에 달성할 수 없다는 Brewer의 추측입니다. 이 추측은 Seth Gilbert와 Nancy Lynch에 의해 2002년, [Brewer’s Conjecture and the Feasibility of Consistent, Available, Partition-Tolerant Web Services](https://dl.acm.org/doi/10.1145/564585.564601)에서 사실로 증명되었습니다.

> Abstract: When designing distributed web services, there are three properties that are commonly desired: consistency, availability, and partition tolerance. It is impossible to achieve all three. In this note, we prove this conjecture in the asynchronous network model, and then discuss solutions to this dilemma in the partially synchronous model.
>
> 초록: 분산된 웹 서비스를 디자인할 때, 흔히 요구되는 3가지 특성: consistency, availability, and partition tolerance이 있습니다. 이러한 특성들을 모두 달성하기는 불가능합니다. 이 글에서, 우리는 이 추측을 비동기 네트워크 모델에서 증명하고, 부분적으로 동기화된 모델에서 이 딜레마에 대한 해결책을 논의합니다.

