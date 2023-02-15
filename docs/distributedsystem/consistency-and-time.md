# Consistency with Times

## What is consistency?

일관성은 컴퓨터 사이언스에서 다양하게 사용됩니다. 그리고 그 밖에서도 아주 많이 쓰이는 용어이죠. 우리는 이 글에서 좀 더 일관성에 대하여 잘 알아보는 시간을 갖고싶습니다. 일단 일관성을 정의해보아야겠죠. 일관성을 엄밀하게 정의하기 위해서는 어떤 영역에서 부르는지 같이 고려해야합니다.

### Consistency in Transaction Context

트랜잭션이라는 개념은 개발자에게 매우 익숙합니다. 특히 데이터베이스를 공부해보았다면 트랜잭션에 3가지 속성에 대하여 알고있을 것입니다. 우리는 트랜잭션 컨셉을 정리한 Jim Gray의 [The Transaction Concept: Virtues and Limitations](http://jimgray.azurewebsites.net/papers/thetransactionconcept.pdf)의 정의를 인용하면서 일관성을 정의해보겠습니다.

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

(생략)

> 2. Formal Model
> 
> In this section, we will formally define what is meant by the terms consistent, available, and partition tolerant.
> 
> 2.1 Atomic Data Objects
> 
> The most natural way of formalizing the idea of a consistent service is as an atomic data object. Atomic [5], or linearizable [4], consistency is the condition expected by most web services today[^3]. Under this consistency guarantee, there must exist a total order on all operations such that each operation looks as if it were completed at a single instant. This is equivalent to requiring requests of the distributed shared memory to act as if they were executing on a single node, responding to operations one at a time. One important property of an atomic read/write shared memory is that any read operation that begins after a write operation completes must return that value, or the result of a later write operation. This is the consistency guarantee that generally provides the easiest model for users to understand, and is most convenient for those attempting to design a client application that uses the distributed service. See [6] for a more complete definition of atomic consistency.
> 
> ^3: Discussing atomic consistency is somewhat different than talking about an ACID database, as database consistency refers to transactions, while atomic consistency refers only to a property of a single request/response operation sequence. And it has a different meaning than the Atomic in ACID, as it subsumes the database notions of both Atomic and Consistent.
>
> 2. 엄격한 모델
> 
> 이 섹션에서, 우리는 consistent, available, partition tolerant가 어떠한 의미를 지니는지 엄결하게 정의할 것이다.
>
> 2.1 원자적 데이터 객체
> 
> 가장 자연스러운 방법으로 일관된 서비스를 엄격하게 정의하는 아이디어는 원자적 데이터 객체이다. 원자적, 혹은 선형화 가능한 일관성은 많은 웹서비스가 예상하는 조건이다. 이 일관성 보장 하에서, 각 작업이 한 instant에서 완료된 것처럼 항상 모든 operations에 대한 Total Order가 존재해야한다. 이는 분산 공유 메모리의 요청이 단일 노드에서 실행되는 것처럼 작동하여 한 번에 하나씩 작업에 응답하도록 요구하는 것과 같다. 원자적 읽기/쓰기 공유 메모리의 중요한 특성 중 하나는 쓰기 작업이 완료된 후 시작되는 읽기 작업은 해당 값을 반환해야 한다는 것이다. 이는 일반적으로 사용자가 가장 쉽게 이해할 수 있는 모델을 제공하는 일관성 보장이며, 분산 서비스를 사용하는 클라이언트 애플리케이션을 설계하려는 사용자에게 가장 편리합니다. 원자적 일관성에 대한 자세한 정의는 [인용-6번]을 참조하십시오.

(TODO, 좀 더 깔끔하게 번역하기)

정리해보자면, CAP 추측을 증명하는 과정에서 Consistent한 서비스를 정의하는 방법으로 원자적 데이터 객체라는 개념을 사용한다. 이는 한번에 하나의 요청만 처리되는 것과 동일하게 나타나야함을 의미한다. 또한 쓰기 작업이 끝난다음 실행된 읽기는 이전 쓰기가 반영된 값을 반환해야한다. 이러한 정의를 Atomic Consistency, 혹은 선형화 가능성(linearizable)이라고 한다. 그리고 자세한 정의는 스킵하고 있다. 우리는 엄밀한 정의가 보고싶기에 조금 더 찾아본다. [인용 6번]은 [Nancy Lynch. Distributed Algorithms, pages 397–350. Morgan Kaufman, 1996.](https://dl.acm.org/doi/book/10.5555/2821576)으로, CAP 추측을 증명한 Nancy Lynch의 분산 시스템 교과서이다. 이 책의 Chapter 13에서 Atomic Objects에 대한 설명을 하고있다. 이 책은 구글 스칼라에 따르면 Nancy Lynch의 글중에서 가장 인용이 많이 되었으며(6k), 우리는 분산 시스템을 공부하러 온 학생이기에 열심히 교과서를 읽어보자.

> Chapter 13 - Atomic Objects
> 
> In this chapter, our last on the asynchronous shared memory model, we introduce
> atomic objects. An atomic object of a particular type is very much like an
> ordinary shared variable of that same type. The difference is that an atomic
> object can be accessed concurrently by several processes, whereas accesses to
> a shared variable are assumed to occur indivisibly. Even though accesses are
> concurrent, an atomic object ensures that the processes obtain responses that
> make it look like the accesses occur one at a time, in some sequential order that
> is consistent with the order of invocations and responses. Atomic objects are
> also sometimes called linearizable objects. 
>
> 챕터 13 - 원자적 객체
>
> 이 챕터에서, 우리는 마지막 비동기 공유 메모리 모델인 원자적 객체를 소개한다.
> 특정 유형의 원자적 객체는 같은 유형의 일반적인 공유 변수와 매우 유사합니다.
> 원자적 객체의 다른 점은, 여러 프로세스에서 접근이 가능하고 이 접근은 각각(indivisibly) 발생한다고 가정한다는 것이다.
> 심지어 이러한 접근이 동시에 일어난다며느 원자적 객체는 프로세스에서 한번에 하나씩 접근하는 것처럼 보이도록 응답을 처리하는걸 보장한다.

우리는 atomic consistency의 정의를 찾으러왔는데, 갑자기 atomic object에 대한 설명이 나왔다. 물론 틀린 이야기는 아니다. 여기서는 atomic object를 atomic data type(atomic consistency를 만족하는 객체)으로 이해할 수 있다. atomic data type은 atomic한 것과 더해서 Resilient한 것이 추가적으로 요구된다. 이런 설명은 [William Weihl, Barbara Liskov. Specification and Implementation of Resilient, Atomic Data Types](https://dl.acm.org/doi/pdf/10.1145/872728.806851)를 살펴보면 확인할 수 있다.

> 여담으로, atomic data type의 2저자인 리스코프는 리스코프 치환원칙으로 잘 알려져있다.
>
> William Weihl는 Nancy Lynch와 같은 MIT에 속해있었으며, Distributed Algorithms 책에 인용된 [A Theory of Atomic Transactions](https://groups.csail.mit.edu/tds/papers/Lynch/lncs88.pdf)이라는 논문을 같이 작성했다. 그리고 atomic transaction에서는 Atomic Data Types가 인용되어 있다. William Weihl와 Nancy Lynch는 같이 한 다른 연구들도 있는데, 관심이 있다면 [Hybrid atomicity for nested transactions](https://linkinghub.elsevier.com/retrieve/pii/030439759500029V)도 같이 확인해봐도 좋을 것이다.
> 
> 여기에서 shared memory model과 같은 설명이 많이 나오는데, [David Mosberger. Memory Consistency Mode](https://dl.acm.org/doi/pdf/10.1145/160551.160553)를 살펴보면 조금 더 이해가 될 것이다. 이러한 consistency model은 멀티 프로세스 상에서 공유 메모리를 어떻게 접근할 것인지에 대한 내용에서 시작되었다.

#### Atomic Transaction

> The idea of nested transactions seems to have originated in the "spheres of control" work of [Davies].
> Reed [Reed] developed the current notion of nesting and designed a timestamp-based implementation.
> Moss [Moss] later designed a locking implementation that serves as the basis of the implementation of
> the Argus programming language. 
>
> moss: https://apps.dtic.mil/sti/pdfs/ADA100754.pdf

### Consistency Models

### External Consistency

