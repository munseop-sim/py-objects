# chapter05. 책임할당하기

### 이전챕터 내용 정리

- 데이터 중심의 접근법의 문제점 : 행동보다 데이터를 먼저 결정하고 협력에서 벗어가 고립된 객체의 상태에 초점을 맞추기 때문에 캡슐화를 위반하기 쉽다 → 결합도가 높아짐 → 코드변경의 어려움
- 이를 극복하기 위해서는 데이터 보다 책임에 초점을 두고 설계
- 책임
    - 정의
        - 객체가 협력에 참여하기 위해 수행하는 로직(행동)
        - 객체에 의해 정의되는 응집도 있는 행위의 집합
        - 객체의 책임 = 하는 것 (doing) + 아는 것(knowing)
- 협력
    - 정의
        - 객체들이 Application의 기능을 구현하기 위해 수행하는 상호작용
        - 어떤 객체가 다른 객체에게 무엇인가를 요청하는 것
    - 메시지 전송은 객체사이의 협력을 위해 사용할 수 있는 유일한 커뮤니케이션 수단
    - 협력은 객체를 설계하는데 필요한 일종의 문맥(context)를 제공
- 역할
    - 정의
        - 객체들이 수행하는 책임들의 집합
        - 역할은 다른것으로 교체할 수 있는 책임의 집합
        - 역할은 객체가 참여할 수 있는 일종의 슬롯
        - 프레임워크나 디자인패턴과 같이 재사용 가능한 코드나 설계 아이디어를 구성하는 핵심적인 요소
    - 역할을 대체할 클래스들 사이에 구현의 공유에 따라 추상클래스, 인터페이스를 이용할 수 있다.



### 책임중심의 설계 원칙

1. 데이터보다 행동을 먼저 결정
    1. **“데이터를 처리하는데 필요한 오퍼레이션이 무엇인가“**에 대해서 고민하기 전에 **객체가 수행해야 하는 책임은 무엇인가를 먼저 결정**하고 객체의 상태(데이터)에 대해 고민한다.
2. 협력이라는 문맥안에서 책임을 결정
    1. 객체에게 할당된 책임이 협력에 어울리지 않다면 그 책임은 나쁜것 → 책임은 객체의 입장이 아니라 객체가 참여하는 협력에 적합해야 한다.
3. 책임주도 설계 → 책임을 결정한 후에 책임을 수행할 객체를 결정하는 것
    1. 시스템이 사용자에게 제공해야 하는 기능인 시스템 책임을 파악
    2. 시스템 책임을 더 작은 책임으로 분할
    3. 분할된 책임을 수행할 수 있는 적절한 객체 또는 역할을 찾아 책임을 할당
    4. 객체가 책임을 수행하는 도중 다른 객체의 도움이 필요한 경우 이를 책임질 적절한 객체 도는 역할을 찾는다.
    5. 해당 객체 또는 역할에게 책임을 할당함으로써 두 객체가 협력하게 한다.

### 책임중심 설계

- 순서
    1. **도메인 설계**
        1. 도메인모델의 간단한 개념 및 도메인 간의 관계에 대한 고민
        2. 도메인 정의에 대해서는 정답이 없다. 실용적이면서 유용한 모델이 정답이라고 할 수 있다.
        3. 구현을 가이드 할 수 있는 도메인모델을 선택한다.
        4. 도메인의 구조가 코드의 구조를 이끈다.
    2. **메세지를 전송할 객체가 원하는 바와 수신하기에 적합한 객체를 선택**
        1. 객체가 상태와 행동을 통합한 캡슐화의 단위에 집중
        2. 객체에게 책임을 할당하는 첫 번째 원칙은 책임을 수행할 정보를 알고 있는 객체에게 책임을 할당 →정보전문가 (Information Expert) 패턴
    3. **구현을 통한 검증**
        1. 1,2 단계를 통한 설계를 바탕으로 구현을 진행하도록 한다.
        2. 변경에 취약한 코드 위험 징후 → 일반적으로 응집도가 낮은 클래스는 아래 3가지 이유를 동시에 가지는 경우가 대부분
            1. 코드를 수정해야 하는 이유를 하나 이상 가지는 클래스 → 변경의 이유에 따라 클래스를 분리한다.
            2. 인스턴스 변수가 초기화 되는 시점 확인 : 응집도가 높은 클래스는 인스턴스 생성시에 모든 속성을 함께 초기화한다. 반면에 응집도가 낮은 클래스는 객체의 속성중 일부만 초기화하고 일부는 초기화하지 않은 상태로 남겨둔다. → 함께 초기화 되는 속성을 기준으로 코드를 분리
            3. 메서드들이 인스턴스 변수를 사용하는 방식 확인 : 모든 메서드가 모든 속성을 사용한다면 클래스의 응집도가 높다고 할 수 있으나, 반대의 상황은 그렇지 않다. → 속성그룹과 해당 그룹에 접근하는 메서드그룹을 기준으로 코드를 분리.
        3. 코드를 분리하여 또 다른 결합이 생성된다면 다형성을 통한 분리를 고려해야 한다.
    4. 유연성에 대한 고민과 변경에 대한 대비
        1. 변경의 대비책
            1. 코드를 이해하고 수정하기 쉽도록 최대한 단순하게 설계
            2. 코드를 수정하지 않고도 변경을 수용할 수 있도록 코드를 더 유용하게 만드는 것
            3. 대부분의 경우 ⅰ방법이 더 좋은 방법이기는 하나, 유사한 변경이 반복적으로 발생한다면 복잡성이 상승하더라도 ⅱ방법으로 유연성을 추가하는 방법이 더 좋다. → 합성을 이용
               cf) 합성 : 인터페이스에 정의된 메세지를 통해서만 코드를 재사용하는 방법
        2. 유연성 = 의존성 관리의 문제

### 책임주도 설계의 대안

- 최대한 빠르게 목적한 기능을 수행하는 코드를 작성하고 난 후에 코드상에 드러나는 책임들을 올바른 위치로 이동 시키는 것. 이 때 중요한 점은 클래스의 겉으로 드러나는 동작이 바뀌어서는 안된다. → 리팩토링
- 책임주도 설계에 익숙하지 않다면 데이터 중심으로 이를 구현한 후에 리팩토링 과정을 통해 책임주도 설계와 유사한 구조를 가지도록 변경을 꾀할 수 있다.
- 긴 메서드의 단점
    - 어떤 일을 수행하는지 한 눈에 파악하기 어렵기 때문에 전체적으로 이해하는데 많은 시간이 필요
    - 너무 많은 작업을 처리하기 때문에 변경이 필요할 때 수정해야 할 부분을 찾기 어려움
    - 메서드 내부의 일부만 수정해도 나머지 부분에서 버그가 발생할 확률이 높음
    - 로직의 일부만 재사용 불가
    - 복사/붙여넣기 이외에 코드의 재사용 방법이 없음 → 코드중복 초래
    - 응집도가 낮기 때문에 이와 같은 결과가 나타는데 이런 메서드를 ‘몬스터 메서드’라고도 한다.
    - 메서드의 길이가 반드시 나쁜 것은 아니다. 중요한 것은 메서드의 이름과 몸체의 의미적 차이
- 긴 메서드의 분해
    - 긴 메서드를 메서드의 책임을 의미하도록 알아보기 쉬운 이름으로 분해한다.
    - 코드를 작은 메서드들로 분해하면 전체적은 흐름을 이해하기 쉬워짐
    - 각 메서드는 단하나의 이유에 의해서만 변경될 수 있다.
    - 작고, 명확하며, 한 가지 일에 집중하는 응집도 높은 메서드는 변경 가능한 설계를 이끌어내는 기반이 도니다.
    - 작게 나눠진 메서드들을 적당한 위치로 분배 (적당한 위치 : 각 메서드가 사용하는 데이터를 정희하고 있는 클래스)
- 자율적인 객체로의 변경 : 긴 메서드를 분해하여, 자율적인 객체에게 책임을 할당 → 자율적인 객체 : 자신이 소유하고 있는 데이터를 자기 스스로 처리



### GRASP (General Responsibility Assignment Software Pattern) 패턴

1. 정의
    1. 객체지향 디자인의 핵심은 각 객체에 책임을 부여하는 것
    2. 책임을 부여하는 원칙들을 말하고 있는 패턴
    3. 구체적은 구조는 없지만 철학을 배울 수 있고 총 9가의 원칙을 가지고 있다.
2. **INFORMATION EXPERT 패턴**
    1. 객체에게 책임을 할당 할 때, 가장 기본이 되는 책임할당 원칙
    2. 책임을 수행하는데 필요한 정보를 가지고 있는 객체에게 할당한다.
    3. 정보를 ‘알고’ 있다고 해서 꼭 ‘저장’하고 있을 필요는 없다.
    4. *예제코드에서 Screening 클래스가 ‘상영’이라는 도메인 모델의 정보전문가에 해당된다. 또한 Movie클래스는 영화가격을 계산할 때  ‘영화’라는 도메인 모델의 정보전문가가 된다. 그리고 ‘할인여부’에 대한 정보전문가 도메인 모델은 ‘할인조건’에 해당된다. 정리하면 Screeing → Movie → DiscountCondition의 흐름으로 메세지를 전송하게 된다.*
3. **LOW COUPLING 패턴**
    1. 의존성을 낮추고 변화에 대한 영향을 줄이며 재사용성을 높이기 위한 고민에서 시작
    2. 전체적인 결합도가 낮게  유지되도록 책임 할당
    3. *예제코드에서 ‘할인조건'에 해당하는 모델은 ‘상영’ 모델과도 결합 할 수 있지만 이렇게 되면 또 다른 결합도가 추가되기 때문에 Low Coupling관점에서는 결합하지 않는 것이 옳다.*
4. **HIGH COHESION 패턴**
    1. 복잡성을 관리하기 위한 수준으로 유지하기 위한 고민에서 시작.
    2. 높은 응집도를 유지 할 수 있게 책임을 할당한다.
    3. *‘상영’모델과 ‘할인조건’ 모델이 협력하게 된다면, ‘상영’모델은 영화요금 계산에 관련된 책임 일부를 떠안아야 한다. 이 때 ‘할일조건’모델이 변경되면 ‘상영’ 모델 또한 변경될 가능성이 높아진다. 따라서 High Cohesion관점에서는 두 모델간 협력하지 않는 것이 옳다.*
5. **CREATOR 패턴**
    1. 어떤방식으로든 생성되는 객ㅊ와 연결되거나 관련될 필요가 있는 객체에 해당 객체 생성에 대한 책임을 위임하는 것
    2. 객체A를 생성하는 객체B를 정하기 위한 조건
        1. B가 A객체를 포함하거나 참조
        2. B가 A객체를 기록
        3. B가 객체를 긴밀하게 사용
        4. B가 A객체를 초기화하는데 필요한 데이터를 가지고 있음( 이 경우 B는 A에 대한 정보전문가 )
6. **POLYMORPHISM 패턴**
    1. 객체의 타입에 따라 변하는 로직이 있을 때 로직을 담당할 책임할당에 관한 고민에서 시작
    2. 하나의 클래스가 여러 타입의 행동을 구현하고 있다면, 클래스를 분해하고 책임을 분산시킨다.
    3. if문, switch문과 같은 조건논리로 코드가 분기되도록 설계가 되었다면 새로운 변화가 일어난 경우 기존 코드를 수정해야 한다.  → 다형성을 이용해서 새로운 변화에 대해 쉽게 확장
7. **PROTECTED VARIATIONS 패턴**
    1. 변화와 불안정성이 다른 요소에 나쁜영향을 미치지 않도록 방지하는 것에 대한 고민에서 시작
    2. 설계에서 변하는 것이 무엇인지 고려하고 변하는 개념에 대해서 캡슐화 → 변경될 가능성이 높다면 캡슐화를 진행하여 개선한다.
    3. 변경될 여지가 있는 곳에 안정된 인터페이스를 정의해서 사용
8. Controller 패턴
    1. 시스템 이벤트(사용자의 요청)를 처리할 객체를 만들자. 시스템/서브시스템으로 들어오는 외부 요청을 처리하는 객체를 만들어 사용. 만약 어떤 시스템안에 있는 각 객체의 기능을 사용할 때, 직접적으로 각 객체에게 접근하게 된다면 서스시스템과 외부간의 결합도가 증가하게 되고 서브시스템의 어떤 객체를 수정할 경우, 외부에 주는 충격이 크게 된다. 서브시스템을 사용하는 입장에서 본다면 이 Controller객체만 알고 있으면 되므로 사용하기 쉽다.
9. Pure Fabrication 패턴
    1. -
10. Indirection 패턴