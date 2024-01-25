## 인덱스의 개념
인덱스란 데이터의 저장(INSERT, UPDATE, DELETE) 의 성능을 희생하고 그 대신에 데이터의 읽기 속도를 높이는 테이블의 동작속도(조회)를 높여주는 자료구조이다. 

## B-tree(밸런스 트리) - MySQL에서 가장 일반적으로 사용되는 인덱스 유형
### 기본 알고리즘
B-tree는 매우 효율적인 탐색을 가능케 하는 밸런스 트리 기반의 알고리즘입니다. 이진 트리의 일반화된 형태로, 각 노드에는 여러 개의 키와 해당 키에 대응하는 값 또는 포인터가 저장됩니다.

- 트리의 높이가 다른 경우 약간의 차이는 있을 수 있지만 O(logN)의 시간을 가집니다.
- 칼럼의 값을 변형하지 않고, 원래의 값을 이용해 인덱싱하는 알고리즘이다.
- 최상위에 하나의 루트 노드가 존재하고 그 하위에 자식 노드가 붙어있는 형태이다.
- 트리 구조의 가장 하위에는 리프 노드라고 하고 트리구조에서 루트노드도 아니고 리프노드도 아닌 중간의 노드를 브랜치 노드라고 한다. 
- 이 인덱스의 최대 장점은 어떤 데이터를 조회하든지, 이에 사용하는 조회 과정의 길이 및 비용이 균등 하다는데 있다.
- 단, 어떤 데이터를 조회 하든지 Root 에서 부터 Leaf 페이지를 모두 거처야 하기 때문에 데이터가 적은 테이블등의 단순 조회로 데이터를 조회하는 과정이 대비 조회 속도가 느린 단점이 있다.

### 인덱스 유형
클러스터 인덱스, 보조 인덱스 등이 B-tree 기반입니다.

## 클러스터 인덱스 (Primary Index)
### 클러스터 인덱스는 하나의 테이블 당 하나만 존재
각 테이블은 하나의 클러스터 인덱스만 가질 수 있습니다. 클러스터 인덱스가 없으면 데이터는 보통 테이블에 삽입된 순서대로 저장됩니다.

### 데이터 정렬 및 검색 속도 향상
클러스터 인덱스를 사용하면 해당 컬럼에 대해 정렬된 상태로 데이터가 저장되므로, 검색 속도가 향상됩니다.

### 기본 키로 자주 사용
주로 기본 키(primary key)에 대해서 클러스터 인덱스를 생성합니다. 기본 키는 각 레코드를 고유하게 식별하므로, 클러스터 인덱스를 이용한 정렬이 레코드의 물리적인 저장 순서와 일치하기에 적합합니다.

### 입력 및 갱신 성능 영향
클러스터 인덱스를 사용하면 데이터의 물리적인 순서에 따라 정렬되기 때문에, 새로운 데이터의 삽입이나 해당 컬럼의 값을 갱신할 때 성능 이슈가 발생할 수 있습니다.

## 효율적인 인덱스 설계 및 사용
- WHERE 절에 사용되는 열 (WHERE 절에 사용되는 열이라도 자주 사용해야 가치가 있음)
- SELECT 절에 자주 등장하는 컬럼들을 잘 조합해서 INDEX로 만들어두면 INDEX 조회 후 다시 데이터에서 조회할 필요가 없으므로 빠르게 검색이 가능합니다.
- JOIN 절에 자주 사용되는 열에는 인덱스의 효율이 좋습니다.
- ORDER BY 절에 사용되는 열은 데이터 페이지가 자동 정렬됐기 때문에 클러스터형 인덱스가 유리합니다.

## 인덱스를 사용할 때 주의할 점
- 데이터 변경(삽입, 수정, 삭제) 작업이 얼마나 자주 일어나는지 고려해야 함.
- 단일 테이블에 인덱스가 많으면 속도가 느려질 수있다. (테이블당 4~5개 권장)
- 검색할 데이터가 전체 데이터의 20% 이상이라면, MySQL에서 인덱스를 사용하지 않음. (강제로 사용할 시 성능 저하를 초래할 수 있음)전체 페이지의 대부분을 읽어야 하고, 인덱스 관련 페이지도 읽어야 해서 작업량이 크기 때문이다.
- 사용하지 않는 인덱스는 제거하는 것이 바람직함. (실무에서 사용하지 않는 보조 인덱스를 몇개 삭제했을 때 성능이 향상되는 경우도 많음)
- 클러스터형 인덱스는 테이블당 하나만 생성할 수 있음
- 테이블에 클러스터형 인덱스가 아예 없는 것이 좋은 경우도 있음