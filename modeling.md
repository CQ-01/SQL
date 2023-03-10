# 1. 데이터 모델링의 이해
## 스키마
- 내부 스키마 : 물리적 저장정보를 나타냄
<br/>
  <span style = "color : red">$\updownarrow$ 물리적</span>
- 개념 스키마 (개념적) : 전체 DB
<br/>
  <span style = "color : red">$\updownarrow$ 논리적</span>
- 외부 스키마 : 개인적 DB

## ERM 모델
> 피터첸, Entity Relationship Model
- ERD : Entity relationship diagram, ERM모델의 산출물
  - 엔티티 그림, 엔티티 배치, 엔티티 관계설정, 관계명 기술, 관계의 참여도 기술, 관계 필수여부
### Entity
> 유용한 정보를 저장하고 관리하기 위한 집합적 존재
- 복수의 인스턴스로 구성
- 복수의 속성 보유
- 유무형에 따른 분류
  - 유형(물리적 형태), 개념(개념적 정보), 사건(업무수행시)
- 발생시점에 따른 분류
  - 기본 : 원래 존재하는 업무, 고유 주식별자
  - 중심 : 다른 엔티티와의 관계로 행위 엔티티 생성
  - 행위 : 자주 바뀌거나 양이 증가
  <br/>
  <span style = "color : green">\* 기본 $\rightarrow$ 중심 $\rightarrow$ 행위</span>

### Attribute
> 인스턴스로 관리하고자 하는 의미상 분리되지 않는 최소의 데이터 단위
- 기본속성 : 업무로부터 추출한 모든 일반적인 속성
- 설계속성 : 업무를 규칙화하기 위해 새로 만들거나 변형, 정의하는 속성
- 파생속성 : 다른 속성에 영향을 받아 발생하는 속성
- 도메인 : 속성에 대한 데이터타입, 크기, 제약사항 지정

### Relationship
> 엔티티의 인스턴스 사이의 논리적인 연관성, 관계 페어링의 집합
<br/>
> 2개의 엔티티 사이에 관심있는 연관규칙이나 정보의 조합이 발생하는가?
- 페어링 : 엔티티 안에 인스턴스가 개별적으로 관계를 가지는 것
- 연관관계(실선) : 항상 이용하는 관계
- 의존관계(점선) : 상대 행위에 의해 발생하는 관계
- 관계차수 : 1대1, 1대다, 다대다
- 관계선택성 : 필수관계, 선택관계

### 식별자
> 엔티티 내에서 인스턴스를 구분하는 구분자, 논리적 단계에 사용
> <br/>
> \* Key는 물리적 단계에 사용
- 대표성여부에 따른 분류
  - 주식별자 : 엔티티 내 각 어커런스를 구분할 수 있음, 타 엔티티와 참조관계 연결
    <span style = "color : green">\* 해당업무에서 자주 이용되는 속성일것, 이름으로 기술되는 것은 no, 복합으로 구성할 경우 너무 많은 속성을 포함하지 않는다</span>
  - 보조식별자 : 대표성 없는 구분자, 타 엔티티와 참조관계 연결하지 않음
- 스스로 생성여부에 따른 분류
  - 내부 : 스스로 생성되는 식별자
  - 외부 : 타 엔티티로부터 받아오는 식별자
- 속성의 수에 따른 분류
  - 단일 : 하나의 속성으로 구성
  - 복합 : 2개 이상의 속성으로 구성
- 대체여부에 따른 분류
  - 본질 : 업무에 의해 만들어지는 식별자
  - 인조 : 인위적으로 만든 식별자
#### 식별자 관계
- 주식별자
  - 부모로부터 받은 식별자를 자식엔티티의 주식별자로 이용
  - 강한 연결관계, 실선 표기
  - 주식별자 관계로만 설정 시 주식별자 증가로 오류 유발
- 비식별자
  - 부모속성을 자식의 일반속성으로 사용
  - 부모없는 자식이 생성될 수 있는 경우
  - 부모와 자식의 생명주기가 다른 경우
  - 여러개의 엔티티가 하나로 통합되었는데 각각의 엔티티가 별도의 관계를 가진 경우
  - 자식엔티티에 별도의 주식별자를 생성하는 것이 더 유리한 경우
  - SQL문장이 길어져 복잡성이 증가되는 것 방지
  - 약한 연결관계, 점선 표기
  - 비식별자 관계로만 설정 시 부모 엔티티와 조인하여 성능 저하

# 2. 데이터 모델과 성능
## 성능 데이터 모델링
> 정규화, 반정규화, 테이블통합, 테이블분할, 조인구조, PK, FK 등 성능 관련 사항이 데이터 모델링에 반영될 수 있도록 하는 것
- 분석/설계(초기) 단계에서 성능을 고려한 모델링을 수행하여 재업무 비용 최소화
- 성능 데이터 모델링 고려사항
  - 1. 데이터 모델링 시 정규화를 정확하게 수행
  - 2. DB 용량산정을 수행
  - 3. DB에 발생되는 트랜잭션의 유형을 파악
  - 4. 용량과 트랜잭션의 유형에 따라 반정규화를 수행
  - 5. 이력모델의 조정, PF/FK 조정, 슈퍼/서브타입 조정
  - 6. 성능관점에서 데이터 모델을 검증
  
## 정규화
> 반복적인 데이터를 분리하고 각 데이터가 종속된 테이블에 적절하게 배치되도록 하는 것
- 1차 정규화 : 같은 성격, 내용 컬럼이 연속될 때, 컬럼을 제거하고 테이블을 생성
- 2차 정규화 : PK 복합키 구성일 때 부분적 함수 종속 관계 테이블 분리
- 3차 정규화 : PK가 아닌 일반 컬럼에 의존하는 컬럼 분리

## 반정규화
> 정규화된 엔티티, 속성, 관계에 대해 
## 분산 DB