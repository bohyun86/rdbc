MySQL에서 `ROW_NUMBER()`, `RANK()`, `DENSE_RANK()` 같은 윈도우 함수를 사용하는 방법에 대해 예시와 함께 설명해드리겠습니다.

### 1. `ROW_NUMBER()`
`ROW_NUMBER()`는 결과 집합 내에서 각 행에 대해 고유한 순번을 할당합니다. 이 함수는 데이터셋을 정렬하고 순차적으로 행 번호를 부여하고 싶을 때 유용합니다.

```sql
SELECT 
    employee_id,
    name,
    salary,
    ROW_NUMBER() OVER (ORDER BY salary DESC) AS row_num
FROM 
    employees;
```
- **설명**: 
  - `ROW_NUMBER()`는 `ORDER BY salary DESC`에 따라 급여가 높은 순으로 각 행에 고유한 번호를 부여합니다.
  - `row_num`이라는 새로운 열로 결과에 나타납니다.
  
### 2. `RANK()`
`RANK()`는 같은 값을 가지는 행들에 동일한 순위를 부여하고, 다음 순위는 건너뛰는 방식을 취합니다. 중복된 값이 있을 때 그 다음의 순위가 연속되지 않고 건너뛰는 특징이 있습니다.

```sql
SELECT 
    employee_id,
    name,
    salary,
    RANK() OVER (ORDER BY salary DESC) AS rank
FROM 
    employees;
```
- **설명**:
  - `RANK()`는 급여가 같은 직원에게 동일한 순위를 부여합니다.
  - 동일한 순위가 부여된 뒤에는 순위가 건너뛰어 다음 순위가 매겨집니다.
  - 예를 들어, 급여가 5000인 직원이 두 명이면 두 명 모두 1위를 차지하고 그다음 순위는 3위로 시작합니다.
  
### 3. `DENSE_RANK()`
`DENSE_RANK()`는 중복된 값이 있을 때 순위를 건너뛰지 않고 계속합니다. 이는 `RANK()`와는 다른 점입니다.

```sql
SELECT 
    employee_id,
    name,
    salary,
    DENSE_RANK() OVER (ORDER BY salary DESC) AS dense_rank
FROM 
    employees;
```
- **설명**:
  - `DENSE_RANK()`도 마찬가지로 급여 순으로 직원들에게 순위를 매기지만, 순위가 건너뛰지 않습니다.
  - 만약 급여가 같은 직원이 두 명이 있다면 둘 다 1위를 받고, 그다음은 2위로 진행됩니다.

### 윈도우 함수의 일반적인 형태
윈도우 함수들은 `OVER()` 절을 사용해 특정 기준에 따라 데이터를 나누고 정렬할 수 있습니다. `OVER()` 절 안에서는 `PARTITION BY`를 사용해 특정 그룹별로 순위를 매길 수도 있습니다.

예를 들어, 각 부서별로 급여 순위를 매기고 싶다면:

```sql
SELECT 
    department_id,
    employee_id,
    name,
    salary,
    ROW_NUMBER() OVER (PARTITION BY department_id ORDER BY salary DESC) AS department_rank
FROM 
    employees;
```
- **설명**:
  - `PARTITION BY department_id`를 사용하면 부서별로 행을 나누어서 순위를 매깁니다.
  - `ORDER BY salary DESC`에 따라 부서 내에서 급여가 높은 순으로 순위를 부여합니다.

### 결론
- **`ROW_NUMBER()`**: 고유한 순번을 할당하며, 중복된 값이 있어도 항상 연속적인 번호를 부여.
- **`RANK()`**: 동일한 값이 있을 경우 동일한 순위를 부여하고, 다음 순위는 건너뜀.
- **`DENSE_RANK()`**: 동일한 값이 있을 경우 동일한 순위를 부여하지만, 다음 순위를 건너뛰지 않음.

윈도우 함수를 통해 데이터를 정렬하거나 그룹화하여 분석하는 데 유용한 기능을 제공하며, 각 함수의 차이를 이해하고 적절히 사용하는 것이 중요합니다.