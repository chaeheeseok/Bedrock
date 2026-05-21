# 코드 리뷰 리포트

> 생성 일시: 2025-05-21  
> 사용 모델: Claude Sonnet 4.6  
> 검사 항목: PEP8 스타일 · OWASP 보안 취약점

---

## order_manager.py (147줄)

### 스타일 검사

PEP8 및 일반적인 Python 스타일 규칙 위반 목록:

**명명 규칙 (Naming Convention)**
- [9] 스타일 위반: 클래스명 `orderManager`가 PascalCase를 따르지 않음 → `OrderManager`로 변경 필요
- [14] 스타일 위반: 메서드명 `CreateOrder`가 snake_case를 따르지 않음 → `create_order`로 변경 필요
- [29] 스타일 위반: 메서드명 `CalcDiscount`가 snake_case를 따르지 않음 → `calc_discount`로 변경 필요
- [55] 스타일 위반: 클래스명 `reportBuilder`가 PascalCase를 따르지 않음 → `ReportBuilder`로 변경 필요
- [69] 스타일 위반: 메서드명 `GenerateTextReport`가 snake_case를 따르지 않음 → `generate_text_report`로 변경 필요

**코드 스타일**
- [30] 스타일 위반: `if total == None:` → `if total is None:`으로 작성하는 것이 Pythonic함
- [36] 스타일 위반: `def get_order(self,order_id)` — 쉼표 뒤 공백 누락 → `def get_order(self, order_id)`
- [37] 스타일 위반: `self.orders.get(order_id,None)` — 쉼표 뒤 공백 누락 → `self.orders.get(order_id, None)`
- [47] 스타일 위반: `def update_status(self,order_id,status)` — 쉼표 뒤 공백 누락
- [48~52] 스타일 위반: `if ... return True else: return False` → `return order_id in self.orders` 후 상태 업데이트로 early return 패턴 권장
- [60] 스타일 위반: `def build_html_summary(self,title,content)` — 쉼표 뒤 공백 누락
- [69] 스타일 위반: `def GenerateTextReport(self,manager)` — 쉼표 뒤 공백 누락
- [70] 스타일 위반: `lines=[]` — 대입 연산자 주위 공백 누락 → `lines = []`
- [72] 스타일 위반: `for oid,info in` — 쉼표 뒤 공백 누락 → `for oid, info in`
- [73] 스타일 위반: 문자열 `+` 연결 반복 사용 → f-string 사용 권장 (`f"주문ID: {oid} | 고객: {info['customer']} | 금액: {info['total']}원"`)
- [80] 스타일 위반: `total = total + manager.orders[oid]["total"]` → `total += ...`로 작성하는 것이 Pythonic함
- [81] 스타일 위반: `if len(manager.orders) == 0:` → `if not manager.orders:`로 작성하는 것이 Pythonic함
- [24~26] 스타일 위반: `for oid in self.orders: result.append(self.orders[oid])` → `return list(self.orders.values())`로 간결하게 표현 가능

**보안 위반**
- [6~7] 보안 위반: `DB_PASSWORD = "shop_admin_2024"` 및 `API_SECRET = "sk-live-abc123xyz456"` — 자격증명 하드코딩 (OWASP A02)
- [41] 보안 위반: `search_orders`에서 SQL 쿼리를 문자열 연결로 조합 — SQL Injection 취약점 (OWASP A03)
- [61~62] 보안 위반: `build_html_summary`에서 사용자 입력을 HTML에 직접 삽입 — XSS 취약점 (OWASP A03)
- [66~67] 보안 위반: `debug_credentials`에 하드코딩된 API 키 및 디버그 출력 — 프로덕션 코드에 존재하면 안 됨

**독스트링 누락**
- [9] 스타일 위반: 클래스 `orderManager`에 docstring 없음
- [23] 스타일 위반: `get_all_orders` 메서드에 docstring 없음
- [29] 스타일 위반: `CalcDiscount` 메서드에 docstring 없음
- [55] 스타일 위반: 클래스 `reportBuilder`에 docstring 없음
- [60] 스타일 위반: `build_html_summary` 메서드에 docstring 없음
- [77] 스타일 위반: `calculate_total_revenue` 메서드에 docstring 없음
- [86] 스타일 위반: `login_admin` 함수에 docstring 없음

---

### 보안 검사

코드 분석 결과, SQL Injection · XSS · 하드코딩된 자격증명 등 총 4가지 취약점을 발견했습니다.

---

## 🔴 취약점 1: 하드코딩된 자격증명 (심각도: 높음)

**위치:** 모듈 최상단 (6~7번 줄) 및 `debug_credentials()` (66번 줄)

```python
DB_PASSWORD = "shop_admin_2024"
API_SECRET  = "sk-live-abc123xyz456"
...
def debug_credentials(self):
    secret = "sk-live-abc123xyz456"
    print("DEBUG API KEY:", secret)
```

**문제점:**
- 비밀번호와 API 키가 소스코드에 평문으로 하드코딩되어 있어, GitHub 등 코드 저장소에 업로드될 경우 즉시 탈취 가능합니다.
- `login_admin()`에서 평문 비밀번호를 직접 비교하므로, 비밀번호 노출 시 관리자 권한 탈취가 가능합니다.
- `debug_credentials()`가 운영 환경에서도 실행될 경우 API 키가 로그에 남을 수 있습니다.
- (OWASP A02:2021 – Cryptographic Failures 해당)

**수정 제안:**
```python
import os
import hashlib

# 환경변수에서 읽고, 해시값으로 비교
DB_PASSWORD_HASH = os.environ.get("DB_PASSWORD_HASH")
API_SECRET       = os.environ.get("API_SECRET")

def login_admin(password: str) -> bool:
    input_hash = hashlib.sha256(password.encode()).hexdigest()
    return input_hash == DB_PASSWORD_HASH
```

---

## 🔴 취약점 2: SQL Injection (심각도: 높음)

**위치:** `OrderManager.search_orders()` (41~44번 줄)

```python
def search_orders(self, keyword):
    query = "SELECT * FROM orders WHERE customer_name = '" + keyword + "'"
    conn  = sqlite3.connect("shop.db")
    cur   = conn.cursor()
    cur.execute(query)   # Injection 위험
```

**문제점:**
- 사용자 입력값 `keyword`를 검증 없이 SQL 쿼리에 직접 연결(concatenation)합니다.
- `' OR '1'='1` 입력 시 전체 주문 데이터 탈취가 가능하며, `'; DROP TABLE orders; --` 입력 시 테이블 전체 삭제가 가능합니다.
- (OWASP A03:2021 – Injection 해당)

**수정 제안:**
```python
# Parameterized Query (Prepared Statement) 사용
def search_orders(self, keyword: str) -> list:
    """고객명으로 주문 검색 (SQL Injection 방지)"""
    query = "SELECT * FROM orders WHERE customer_name = ?"
    conn  = sqlite3.connect("shop.db")
    cur   = conn.cursor()
    cur.execute(query, (keyword,))
    return cur.fetchall()
```

---

## 🔴 취약점 3: XSS (Cross-Site Scripting) (심각도: 높음)

**위치:** `ReportBuilder.build_html_summary()` (61~62번 줄)

```python
def build_html_summary(self, title, content):
    html  = "<h1>" + title + "</h1>"
    html  = html + "<table>" + content + "</table>"
    return html
```

**문제점:**
- `title`이나 `content`에 `<script>alert('XSS')</script>` 같은 악성 스크립트가 삽입될 경우 그대로 HTML에 포함됩니다.
- 해당 HTML이 브라우저에서 렌더링되면 악성 스크립트가 실행되며, 세션 탈취·피싱 등 2차 공격으로 이어질 수 있습니다.
- (OWASP A03:2021 – Injection 해당)

**수정 제안:**
```python
import html as html_lib

def build_html_summary(self, title: str, content: str) -> str:
    """HTML 이스케이프 처리 후 요약 생성"""
    safe_title   = html_lib.escape(title)
    safe_content = html_lib.escape(content)
    return f"<h1>{safe_title}</h1><table>{safe_content}</table>"
```

---

## 🟡 취약점 4: 입력값 검증 미흡 (심각도: 중간)

**위치:** `main()` 함수, choice == "3" 처리 (119번 줄)

```python
oid = int(input("주문 ID: "))   # 숫자가 아닌 값 입력 시 ValueError
```

**문제점:**
- 사용자가 숫자가 아닌 값(예: `abc`)을 입력하면 `ValueError`가 발생하여 프로그램이 비정상 종료됩니다.
- 주문 상태 값에 대한 허용 목록(whitelist) 검증도 없어 임의 문자열이 상태로 저장될 수 있습니다.

**수정 제안:**
```python
VALID_STATUSES = {"pending", "paid", "shipped", "cancelled"}

try:
    oid    = int(input("주문 ID: "))
    status = input("상태 (pending/paid/shipped/cancelled): ")
    if status not in VALID_STATUSES:
        raise ValueError(f"유효하지 않은 상태값: {status}")
    ok = mgr.update_status(oid, status)
    print("변경 완료" if ok else "주문 없음")
except ValueError as e:
    print(f"입력 오류: {e}")
```

---

## 📋 취약점 요약

| # | 취약점 유형 | 위치 | 심각도 |
|---|------------|------|--------|
| 1 | 하드코딩된 자격증명 | 모듈 상단 6~7줄, `debug_credentials` | 🔴 높음 |
| 2 | SQL Injection | `search_orders` (41~44줄) | 🔴 높음 |
| 3 | XSS | `build_html_summary` (61~62줄) | 🔴 높음 |
| 4 | 입력값 검증 미흡 | `main()` (119줄) | 🟡 중간 |

> 특히 1~3번 취약점은 OWASP Top 10에 포함된 대표적인 보안 취약점으로, 즉시 수정이 필요합니다.

---
