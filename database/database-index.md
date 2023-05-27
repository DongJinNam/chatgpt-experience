# index

### 데이터베이스 인덱스 종류 및 활용 예시

**B-Tree 인덱스**
```
가장 일반적으로 사용되는 인덱스 유형입니다.
데이터베이스에서 범위 검색과 정렬에 효과적입니다.
예시: 사용자 테이블에서 이름으로 검색하는 경우, 이름 열에 B-Tree 인덱스를 생성하여 이름 순으로 빠르게 검색할 수 있습니다.
```

**해시 인덱스**

```
해시 함수를 사용하여 키-값 쌍을 저장하고 검색하는 데 사용됩니다.
정확한 값으로 검색하는 데에는 효과적이지만 범위 검색에는 적합하지 않습니다.
예시: 사용자 테이블에서 고유한 ID로 빠르게 검색하는 경우, ID 열에 해시 인덱스를 생성하여 고유한 ID 값을 기반으로 빠르게 검색할 수 있습니다.
```

**클러스터형 인덱스**

```
테이블의 레코드를 물리적으로 정렬하는 인덱스입니다.
특정 열에 대한 검색과 함께 연속된 레코드에 대한 범위 검색에 효과적입니다.
예시: 주문 테이블에서 날짜 열을 기반으로 주문을 검색하고 날짜 순으로 정렬하는 경우, 날짜 열에 클러스터형 인덱스를 생성하여 효율적인 검색 및 정렬을 수행할 수 있습니다.
```

**전문 텍스트 인덱스**

```
텍스트 기반 열에서 키워드 검색을 수행하는 데 사용됩니다.
자연어 처리 알고리즘을 사용하여 텍스트를 색인화하고 효과적인 키워드 검색을 제공합니다.
예시: 게시물 테이블에서 제목이나 내용에 대한 키워드 검색을 수행하는 경우, 제목 및 내용 열에 전문 텍스트 인덱스를 생성하여 빠르게 관련 게시물을 검색할 수 있습니다.
```

**공간 인덱스**

```
지리 정보나 공간 데이터를 다루는 열에 대한 검색을 효율적으로 수행하는 데 사용됩니다.
공간 데이터의 위치, 거리, 인접성
```

### MySQL 에서 사용하는 주요 인덱스 종류

**B-Tree 인덱스**

```
MySQL에서 가장 일반적으로 사용되는 인덱스 유형입니다.
데이터를 B-Tree 구조로 저장하여 검색 및 정렬에 효율적입니다.
데이터베이스의 대부분의 열에 B-Tree 인덱스를 생성할 수 있습니다.
```

**해시 인덱스**

```
MySQL 8.0부터 지원되는 인덱스 유형입니다.
해시 함수를 사용하여 데이터를 해시 테이블에 저장하고 검색합니다.
정확한 값으로 검색하는 데에 효과적이지만, 범위 검색에는 적합하지 않습니다.
메모리 내에 적은 공간을 사용하여 빠른 검색 성능을 제공합니다.
```

**전문 텍스트 인덱스**

```
FULLTEXT 인덱스라고도 불리며, 텍스트 열에서 키워드 기반의 검색을 지원합니다.
MySQL의 MyISAM 및 InnoDB 스토리지 엔진에서 사용할 수 있습니다.
전문 텍스트 검색 함수(FULLTEXT search functions)를 사용하여 텍스트 열을 검색합니다.
```

**공간 인덱스**

```
MySQL에서 지리 정보와 공간 데이터를 다루는 열에 대한 검색을 지원하는 인덱스입니다.
공간 데이터의 위치, 거리, 인접성 등을 기반으로 검색 및 분석할 수 있습니다.
MySQL의 MyISAM 및 InnoDB 스토리지 엔진에서 지원합니다.
```

**칼럼스토어 인덱스**

```
MySQL 8.0부터 지원되는 인덱스 유형입니다.
칼럼스토어(ColStore) 엔진에 사용되며, 열 지향 데이터 저장 방식을 활용합니다.
대량의 열 데이터를 효율적으로 압축하고 검색 성능을 향상시킬 수 있습니다.
```

### 인덱스 스캔 방식

1. 전체 스캔 (Full Scan):

- 전체 스캔은 데이터베이스의 모든 레코드를 순차적으로 읽어오는 방식입니다.
- 데이터베이스 시스템은 디스크에서 블록 단위로 데이터를 읽어와서 처리합니다.
- 전체 스캔은 인덱스가 없거나 전체 데이터에 대한 연산을 수행할 때 사용될 수 있습니다.
- 하지만 큰 데이터베이스의 경우 성능 저하가 발생할 수 있으므로 인덱스를 활용하는 것이 선호됩니다.

2. 인덱스 스캔 (Index Scan):

- 인덱스 스캔은 인덱스를 사용하여 데이터를 검색하는 방식입니다.
- 인덱스는 키와 해당 키가 존재하는 데이터 위치를 매핑하는 자료구조입니다.
- 인덱스 스캔은 인덱스 키의 값 또는 범위에 해당하는 데이터를 빠르게 찾는 데 사용됩니다.
- B-Tree 인덱스의 경우, 키의 순서를 기반으로 검색하므로 데이터의 정렬이 유지됩니다.

3. 범위 스캔 (Range Scan):

- 범위 스캔은 인덱스를 사용하여 특정 범위 내의 데이터를 검색하는 방식입니다.
- 인덱스의 범위 스캔은 시작 키와 끝 키 사이의 값에 해당하는 데이터를 검색합니다.
- 범위 스캔은 데이터베이스 시스템에서 BETWEEN, >, <, >=, <= 등의 비교 연산자를 사용하는 쿼리에 유용합니다.

4. 해시 스캔 (Hash Scan):

- 해시 스캔은 해시 인덱스를 사용하여 데이터를 검색하는 방식입니다.
- 해시 인덱스는 해시 함수를 사용하여 키와 해당 키가 존재하는 데이터 위치를 매핑합니다.
- 해시 스캔은 정확한 키 값에 해당하는 데이터를 빠르게 찾는 데 사용됩니다.
- 하지만 해시 스캔은 범위 검색에는 적합하지 않습니다.

### 인덱스 스캔 활용 전제조건

1. 정렬된 데이터 필요:
- 인덱스는 데이터를 정렬된 상태로 유지하므로, 정렬된 데이터를 필요로 하는 쿼리에서 인덱스 스캔은 매우 효율적입니다.
- 예를 들어, 특정 열을 기준으로 정렬된 결과를 필요로 하는 ORDER BY 절이 있는 쿼리에서 인덱스 스캔은 정렬 단계를 생략하고 인덱스의 정렬 순서를 활용하여 빠른 결과 반환을 가능하게 합니다.

2. 특정 범위의 데이터 필요:
- 인덱스 스캔은 범위 스캔을 효율적으로 수행할 수 있습니다.
- 예를 들어, WHERE 절에서 BETWEEN, >, <, >=, <= 등의 비교 연산자를 사용하여 특정 범위의 데이터를 조회하는 경우 인덱스 스캔이 유용합니다.
- 인덱스는 데이터를 정렬된 순서로 유지하므로, 범위 스캔은 인덱스의 순서를 활용하여 빠르게 검색할 수 있습니다.

3. 조인 작업 필요:
- 조인 작업은 두 개 이상의 테이블 간에 데이터를 결합하는 작업입니다.
- 인덱스 스캔은 조인에 사용되는 조인 키를 인덱스로 사용하여 조인 작업을 효율적으로 수행할 수 있습니다.
- 조인 키가 인덱스로 지정되어 있다면, 조인 작업에서 인덱스 스캔을 활용하여 조인 연산을 빠르게 수행할 수 있습니다.

4. 필터링 작업 필요:
- WHERE 절에서 특정 조건을 필터링하는 작업에서 인덱스 스캔은 효율적입니다.
- 예를 들어, 특정 열의 값을 가진 레코드를 검색하는 경우 인덱스 스캔은 해당 값을 가진 레코드를 빠르게 찾을 수 있습니다.

5. 집계 작업 필요:
- 집계 함수(MIN, MAX, SUM, COUNT 등)를 사용하는 집계 작업에서 인덱스 스캔은 효율적입니다.
- 인덱스는 데이터를 정렬된 상태로 유지하므로, 집계 함수는 인덱스의 순서