# Company Infra

우사다 건설은 많은 서비스들을 만들고, 운영하기 위하여 인프라가 마련되어 있습니다.

## Resource Management

인프라는 terraform을 이용한 코드로 관리됩니다. 우리는 이러한 방식을 사용하여 인프라를 누구나 쉽게 접근하고 변경할 수 있도록 하였습니다. Workflow는 GitHub과 잘 연동되어서 다른 페이지를 방문할 필요 없이 수정된 내용을 적용할 수 있습니다.

[Infra as a Code](https://github.com/UsadaPeko/infra-as-a-code) Repository에서는 실제 구현 내용을 확인할 수 있습니다.

Terraform은 분명히 처음 보는 Software Engineer에게는 어색한 도구입니다. 하지만 인프라가 공개되어있지 않고, 전부 Devops에게 부탁해야한다면 Devops팀에게 너무나 큰 의존성이 생기며 SE의 개발 속도도 느려지게 됩니다. 우리는 이러한 것들을 방지하기 위하여 SE가 terraform을 수정할 수 있으며 Devops의 Approve는 받도록 강제하였습니다. SE는 Devops에게 부탁할 수도 있으며, 직접 인프라를 수정하는 것도 가능해집니다.

이러한 자율에 바탕을 둔 일하는 방식이 개발 생산성을 올리고 더 응집도 있는 팀을 구성하는데 도움이 된다고 생각합니다.

## Service Catalog
## Deployment

