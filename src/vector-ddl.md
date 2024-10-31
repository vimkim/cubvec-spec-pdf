# 벡터 DDL

이 섹션은 벡터 타입에 대한 데이터 정의 언어(DDL)를 설명합니다.

```sql
vector -- `vector (2000)` 과 동일

vector(512) -- 512차원 `float32` 벡터

vector(float64, 128) -- 128차원 `float64` 벡터
```

## BNF

```bnf
<declaration> ::= "vector" [ "(" [ <vector_type> "," ] <dim> ")" ]
<vector_type> ::= "float32" | "float64" | "binary"
<dim> ::= <positive_integer>
<positive_integer> ::= {digit}+
```

- `<dim>` 은 양의 정수 값입니다:
  - `<vector_type>` 가 `float32` 일 때 1에서 2000 사이.
  - `<vector_type>` 가 `float64` 일 때 1에서 1000 사이.
  - `<vector_type>` 가 `binary` 일 때 1에서 64000 사이.

## 벡터 타입 사양

- `vector` 데이터 타입은 고정 크기의 다차원 배열로, 부동소수점 숫자 또는 이진값을 저장할 수 있습니다.
- 타입이 지정되지 않은 경우 기본 벡터 타입은 `float32` 입니다.
- 차원은 타입 뒤에 괄호 안에 지정됩니다.

### 차원 제한

- `float32` 타입 벡터의 경우 차원은 1에서 2000 사이여야 합니다.
- `float64` 타입 벡터의 경우 차원은 1에서 1000 사이여야 합니다.
- 이진 벡터의 경우 차원은 1에서 64000 사이여야 합니다.

## 선언 예시

1. **기본 벡터 (float32)**

   ```sql
   vector -- `vector (2000)` 과 동일
   ```

2. **512차원 벡터 (float32)**

   ```sql
   vector(512)
   ```

3. **128차원 벡터 (float64)**

   ```sql
   vector(float64, 128)
   ```

## 예시

### 벡터 열이 있는 테이블 생성

```sql
CREATE TABLE items (
  host_year INT NOT NULL PRIMARY KEY,
  embedding vector(3)
);
```

이 예시는 기본 키 `host_year` 와 3차원 벡터 `embedding` 열을 가진 `items` 라는 테이블을 생성합니다.

### 벡터 열 추가를 위한 테이블 변경

```sql
ALTER TABLE items
ADD COLUMN embedding vector(3);
```

이 예시는 기존 `items` 테이블에 3차원 벡터 `embedding` 열을 추가합니다.

### 추가 예시: 이진 벡터

```sql
CREATE TABLE binary_items (
  item_id INT NOT NULL PRIMARY KEY,
  binary_embedding vector(binary, 512)
);
```

이 예시는 512차원의 이진 벡터를 가진 테이블을 생성합니다.
