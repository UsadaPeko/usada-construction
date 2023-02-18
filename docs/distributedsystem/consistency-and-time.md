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
> 이 섹션에서, 우리는 consistent, available, partition tolerant가 어떠한 의미를 지니는지 엄격하게 정의할 것이다.
>
> 2.1 원자적 데이터 객체
> 
> 가장 자연스러운 방법으로 일관된 서비스를 엄격하게 정의하는 아이디어는 원자적 데이터 객체이다. 원자적, 혹은 선형화 가능한 일관성은 많은 웹서비스가 예상하는 조건이다. 이 일관성 보장 하에서, 각 작업이 한 instant에서 완료된 것처럼 항상 모든 operations에 대한 Total Order가 존재해야한다. 이는 분산 공유 메모리의 요청이 단일 노드에서 실행되는 것처럼 작동하여 한 번에 하나씩 작업에 응답하도록 요구하는 것과 같다. 원자적 읽기/쓰기 공유 메모리의 중요한 특성 중 하나는 쓰기 작업이 완료된 후 시작되는 읽기 작업은 해당 값을 반환해야 한다는 것이다. 이는 일반적으로 사용자가 가장 쉽게 이해할 수 있는 모델을 제공하는 일관성 보장이며, 분산 서비스를 사용하는 클라이언트 애플리케이션을 설계하려는 사용자에게 가장 편리합니다. 원자적 일관성에 대한 자세한 정의는 [인용-6번]을 참조하십시오.
>
> 인용-6번.
> 데이터베이스에서 일관성은 트랜잭션에 대한것을 의미하는 반면, 원자적 일관성은 요청/응답 작업 시퀀스의 속성만을 의미하기 때문에 ACID 데이터베이스에 대해 논의하는 것과는 다소 다르다.
> 그리고 ACID의 원자성의 의미와도 다르게, 원자적 일관성은 데이터베이스의 원자성과 일관성에 대한 개념을 모두 포함한다.

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
> 특정 유형의 원자적 객체는 같은 타입의(eg. int/string) 일반적인 공유 변수와 매우 유사합니다.
> 원자적 객체의 다른 점은, 여러 프로세스에서 접근이 가능하고 이 접근은 각각(indivisibly) 발생한다고 가정한다는 것이다.
> 접근이 동시에 이루어지지만, 원자적 객체는 프로세스가 [호출, 응답 순서]와 일치하는 순서로(sequential order) 접근이 한 번에 하나씩 발생하는 것처럼 보이게 하는 응답을 얻을 수 있도록 보장한다.
> 원자적 객체는 종종 선형화 객체로도 불린다.

우리는 atomic consistency의 정의를 찾으러왔는데, 갑자기 atomic object에 대한 설명이 나왔다. 물론 틀린 이야기는 아니다. 여기서는 atomic object를 atomic data type(atomic consistency를 만족하는 객체)으로 이해할 수 있다. atomic data type은 atomic한 것과 더해서 Resilient한 것이 추가적으로 요구된다. 이런 설명은 [William Weihl, Barbara Liskov. Specification and Implementation of Resilient, Atomic Data Types](https://dl.acm.org/doi/pdf/10.1145/872728.806851)를 살펴보면 확인할 수 있다.


<details>
<summary>여담</summary>

> 여담으로, atomic data type의 2저자인 리스코프는 리스코프 치환원칙으로 잘 알려져있다.
>
> William Weihl는 Nancy Lynch와 같은 MIT에 속해있었으며, Distributed Algorithms 책에 인용된 [A Theory of Atomic Transactions](https://groups.csail.mit.edu/tds/papers/Lynch/lncs88.pdf)이라는 논문을 같이 작성했다. 그리고 atomic transaction에서는 Atomic Data Types가 인용되어 있다. William Weihl와 Nancy Lynch는 같이 한 다른 연구들도 있는데, 관심이 있다면 [Hybrid atomicity for nested transactions](https://linkinghub.elsevier.com/retrieve/pii/030439759500029V)도 같이 확인해봐도 좋을 것이다.
> 
> 여기에서 shared memory model과 같은 설명이 많이 나오는데, [David Mosberger. Memory Consistency Mode](https://dl.acm.org/doi/pdf/10.1145/160551.160553)를 살펴보면 조금 더 이해가 될 것이다. 이러한 consistency model은 멀티 프로세스 상에서 공유 메모리를 어떻게 접근할 것인지에 대한 내용에서 시작되었다.

</details>

#### 선형화가능성의 언어 유래

> 13.5 Bibliographic Notes 
The idea of an "atomic object" appears to have originated with the work of Lamport [181, 182] on read/write atomic objects. Herlihy and Wing [153] extended
the notion of atomicity to arbitrary variable types and renamed it linearizability.
Khnig's Lemma was originally proved by Khnig [170]; a proof appears in Knuth's
book [169]. The canonical wait-free atomic object automaton is derived from the
work of Merritt, described in [3]. The connection between atomic objects and
shared variables is derived from work by Lamport and Schneider [186] and by
Goldman and Yelick [139]. The impossibility of implementing read-modify-write
atomic objects using read/write objects is due to Herlihy [150].
The idea of a snapshot atomic object is due to Afek, Attiya, Dolev, Gafni,
Merritt, and Shavit [3] and to Anderson [11, 12], inspired by the work of
Chandy and Lamport on consistent global snapshots in distributed networks [68].
The snapshot atomic object implementations presented here, both UnboundedSnapshot and BoundedSnapshot, are due to Afek, et al. The handshake strategy
used in the BoundedSnapshot protocol is due to Peterson [240]. A more recent atomic snapshot algorithm, requiring only O (nt~logn) time rather than
quadratic time, has been developed by Attiya and Rachman [26].
> 
> 참고 문헌
원자적 객체에 대한 아이디어는 Lamport의 읽기/쓰기 원자적 객체에 대한 연구에서 비롯된 것으로 보인다.
Herlihy와 Wing은 임의 변수 타입에 대한 원자적이란 개념을 확장하고, 선형화가능성으로 이름을 붙였다.
(그 뒤는 생략)

우리는 Nancy의 atomic object의 참고 문헌들에서 atomicity와 linearizability에 대한 추가적인 정보를 얻을 수 있었다. 당연히 우리는 그 논문도 살펴본다.

> # Linearizability: A Correctness Condition for Concurrent Objects 
> 
> A concurrent object is a data object shared by concurrent processes. Linearizability is a correctness
condition for concurrent objects that exploits the semantics of abstract data types. It permits a high
degree of concurrency, yet it permits programmers to specify and reason about concurrent objects
using known techniques from the sequential domain. Linearizability provides the illusion that each
operation applied by concurrent processes takes effect instantaneously at some point between its
invocation and its response, implying that the meaning of a concurrent object’s operations can be
given by pre- and post-conditions. This paper defines linearizability, compares it to other correctness
conditions, presents and demonstrates a method for proving the correctness of implementations, and
shows how to reason about concurrent objects, given they are linearizable. 
>
> # 선형화가능성: 동시성 객체의 정확성 조건(Correctness Condition)
>
> 동시성 객체는 동시 프로세스(concurrent processes)에 의애 공유되는 데이터 객체다.
선형화가능성은 추상 데이터 유형(ADT)의 문법을(semantics) 이용하는 동시성 객체에 대한 정확성 조건입니다.



#### Atomic Data Type - 과거 atomic에 대한 추가 연구들

우리는 이전에 CAP 정리에서 사용된 atomic object라는 것을 살펴보았다. 그러나 여전히 이 것이 어떻게 consistency를 의미하는지는 조금 명확하지 않다. 따라서 consistency와 atomic object의 관계를 조금 더 찾아보겠다.

William Weihl의 [Specification and Implementation of Resilient, Atomic Data Types](https://dl.acm.org/doi/pdf/10.1145/872728.806851)에서 추가적인 정보를 확인할 수 있다. 

> ABSTRACT
> 
> A major issue in many applications is how to preserve the
consistency of data in the presence of concurrency and hardware
failures. We suggest addressing this problem by implementing
applications in terms of abstract data types with two properties:
Their objects are atomic (they provide serializability and
recoverability for activities using them) and resilient (they survive
hardware failures with acceptably high probability). We define
what it means for abstract data types to be atomic and resilient.
We also discuss issues that arise in implementing such types, and
describe a particular linguistic mechanism provided in the Argus
programming language. 
>
> 초록
>
> 많은 애플리케이션에서 겪고있는 주요한 문제는 동시성과 하드웨어 장애로부터 어떻게 데이터 일관성을 보존하는지이다.
> 우리는 다음 두 가지 속성을 가진 추상적인 데이터 유형(ADT)의 관점에서 애플리케이션을 구현하여 이 문제를 해결할 것을 제안한다.
> 첫번째 속성: 그 객체들은 원자적이다. (그들은 직렬화가능성과(serializability) 활동에 대한 복구 가능성을(recoverability for activities) 제공한다.)
> 두번째 속성: 탄력적(resilient)이다. (그들은 높은 하드웨어 장애 가능성에서도 살아남는다.)
> 우리는 abstract data types이 atomic and resilient하다는 것이 무엇인지 정의한다.
> 또한 우리는 이러한 타입을 구현하면서 생기는 문제에 대하여 논의하고, Argus 프로그래밍 언어에 대한 특정 언어학적 메커니즘을 설명한다.


<details>
<summary>Argus란</summary>
Argus는 바바라 리스코프가 만든 분산 객체지향 언어이다.
</details>

(생략)

> To support consistency it is useful to make the activities that
use and manipulate the data atomic. Atomic activities are referred
to as actions or transactions; they were first identified in work on
databases [5, 6, 8]. An atomic action is distinguished by two
properties, indivisibility and recoverability.
> Indivisibility means that
the execution of one action never appears to overlap (or contain)
the execution of any other action. One way of achieving
indivisibility is to run actions serially. However, greater
concurrency is desirable, provided the concurrent actions do not
interfere with one another. Non-interference can be guaranteed if
the effect of running the actions concurrently is the same as if they
had been executed serially in some order. If this condition is true,
the actions are said to be seria/izab/e [8, 23]. 
>
> 일관성을 지원하기 위하여, 데이터를 조작하고, 이용하는 활동을(activities) 원자적으로 만드는 것이 도움이 된다.
원자적 활동은(activities) 액션이나, 데이터베이스와 관련된 연구에서 최초로 확인된(first identified) 트랜잭션을 지칭한다.
원자적 액션은 indivisibility와 recoverability, 두가지 속성으로 나타난다.
> indivisibility는 하나의 액션의 실행이 또다른 하나의 액션의 실행과 절대 겹쳐서 (혹은 같이) 나타나지 않는다.
indivisibility을 달성하는 한가지 방법은 액션을 순서대로(serially) 실행하는 것이다.
그러나, 동시에 하는 요청이 서로 간섭하지 않는한, 더 많은 요청이 동시에 실행되는 것이 요구된다.
작업을 동시에 실행하는 효과가 순차적으로(serially in some order) 실행된 것과 동일한 경우에는 간섭이 없음을 보장할 수 있습니다.
만약 이러한 조건을 만족한다면 액션은 직렬화 가능하다고(serializable) 할 수 있다.


### Consistency Models

### External Consistency

### Atomic Transaction

> The idea of nested transactions seems to have originated in the "spheres of control" work of [Davies].
> Reed [Reed] developed the current notion of nesting and designed a timestamp-based implementation.
> Moss [Moss] later designed a locking implementation that serves as the basis of the implementation of
> the Argus programming language. 
>
> moss: https://apps.dtic.mil/sti/pdfs/ADA100754.pdf

### Nested Transactions



### Spanner

>These features are enabled by the fact that Spanner assigns globally meaningful
commit timestamps to transactions, even though transactions may be distributed. The
timestamps reflect serialization order. In addition, the serialization order satisfies external consistency (or equivalently, linearizability [Herlihy and Wing 1990]): if a transaction T1 commits before another transaction T2 starts, then T1’s commit timestamp
is smaller than T2’s. Spanner is the first system to provide such guarantees at global
scale
