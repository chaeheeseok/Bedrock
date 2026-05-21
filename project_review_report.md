# 코드 리뷰 리포트

> 생성 일시: 2025-05-21  
> 사용 모델: Claude Sonnet 4.6  
> 검사 항목: PEP8 스타일 · OWASP 보안 취약점

---

## order_manager.py (335줄)

### 스타일 검사

PEP8 및 일반적인 Python 스타일 규칙 위반 목록:

**명명 규칙 (Naming Convention)**
- [22] 스타일 위반: 클래스명 `orderManager`가 PascalCase를 따르지 않음 → `OrderManager`로 변경 필요
- [30] 스타일 위반: 메서드명 `CreateOrder`가 snake_case를 따르지 않음 → `create_order`로 변경 필요
- [40] 스타일 위반: 메서드명 `AddCustomer`가 snake_case를 따르지 않음 → `add_customer`로 변경 필요
- [61] 스타일 위반: 메서드명 `CalcDiscount`가 snake_case를 따르지 않음 → `calc_discount`로 변경 필요
- [68] 스타일 위반: 메서드명 `CalcVipGrade`가 snake_case를 따르지 않음 → `calc_vip_grade`로 변경 필요
- [148] 스타일 위반: 클래스명 `reportBuilder`가 PascalCase를 따르지 않음 → `ReportBuilder`로 변경 필요
- [170] 스타일 위반: 메서드명 `GenerateTextReport`가 snake_case를 따르지 않음 → `generate_text_report`로 변경 필요
- [180] 스타일 위반: 메서드명 `GenerateCustomerReport`가 snake_case를 따르지 않음 → `generate_customer_report`로 변경 필요
- [203] 스타일 위반: 메서드명 `GetStatusSummary`가 snake_case를 따르지 않음 → `get_status_summary`로 변경 필요

**코드 스타일**
- [62] 스타일 위반: `if total == None:` → `if total is None:`으로 작성하는 것이 Pythonic함
- [69] 스타일 위반: `if customer == None:` → `if customer is None:`으로 작성하는 것이 Pythonic함
- [74] 스타일 위반: `total_spent = total_spent + order["total"]` → `total_spent += order["total"]`로 작성하는 것이 Pythonic함
- [81] 스타일 위반: `def get_order(self,order_id)` — 쉼표 뒤 공백 누락
- [84] 스타일 위반: `def get_customer(self,cid)` — 쉼표 뒤 공백 누락
- [100] 스타일 위반: `def update_status(self,order_id,status)` — 쉼표 뒤 공백 누락
- [101~105] 스타일 위반: `if ... return True else: return False` → early return 패턴 권장
- [107] 스타일 위반: `def delete_order(self,order_id)` — 쉼표 뒤 공백 누락
- [158] 스타일 위반: `def build_html_summary(self,title,content)` — 쉼표 뒤 공백 누락
- [164] 스타일 위반: `def build_html_customer_list(self,customers)` — 쉼표 뒤 공백 누락
- [170] 스타일 위반: `lines=[]` — 대입 연산자 주위 공백 누락
- [172] 스타일 위반: `for oid,info in` — 쉼표 뒤 공백 누락
- [173] 스타일 위반: 문자열 `+` 연결 반복 사용 → f-string 사용 권장
- [192] 스타일 위반: `total = total + manager.orders[oid]["total"]` → `total += ...`으로 작성하는 것이 Pythonic함
- [194] 스타일 위반: `if len(manager.orders) == 0:` → `if not manager.orders:`로 작성하는 것이 Pythonic함
- [204] 스타일 위반: `summary[s] = summary[s] + 1` → `summary[s] += 1`로 작성하는 것이 Pythonic함
- [115] 스타일 위반: `get_pending_orders`에서 for문 대신 리스트 컴프리헨션 권장
- [285] 스타일 위반: `round(avg,2)` — 쉼표 뒤 공백 누락

**보안 위반**
- [10~11] 보안 위반: `DB_PASSWORD`, `API_SECRET` 하드코딩 — 자격증명 소스코드 노출 (OWASP A02)
- [88~93] 보안 위반: `search_orders` SQL 쿼리 문자열 직접 연결 — SQL Injection (OWASP A03)
- [95~100] 보안 위반: `search_customers` SQL 쿼리 문자열 직접 연결 — SQL Injection (OWASP A03)
- [120~121] 보안 위반: `ping_customer_server`에서 `subprocess.getoutput` 사용자 입력 직접 전달 — Command Injection (OWASP A03)
- [125~128] 보안 위반: `export_order_file`에서 파일 경로 직접 연결 — Path Traversal (OWASP A01)
- [131~133] 보안 위반: `load_orders_from_file`에서 `pickle.load` 사용 — Insecure Deserialization (OWASP A08)
- [158~162] 보안 위반: `build_html_summary`에서 HTML 이스케이프 없이 출력 — XSS (OWASP A03)
- [164~169] 보안 위반: `build_html_customer_list`에서 고객 정보 HTML 직접 삽입 — XSS (OWASP A03)
- [171~174] 보안 위반: `debug_credentials`에 자격증명 하드코딩 및 출력 — 프로덕션 코드에 존재하면 안 됨

**독스트링 누락**
- [40] 스타일 위반: `AddCustomer` 메서드에 docstring 없음
- [48] 스타일 위반: `get_all_orders` 메서드에 docstring 없음
- [54] 스타일 위반: `get_all_customers` 메서드에 docstring 없음
- [61] 스타일 위반: `CalcDiscount` 메서드에 docstring 없음
- [68] 스타일 위반: `CalcVipGrade` 메서드에 docstring 없음
- [148] 스타일 위반: 클래스 `reportBuilder`에 docstring 없음
- [158] 스타일 위반: `build_html_summary` 메서드에 docstring 없음
- [188] 스타일 위반: `calculate_total_revenue` 메서드에 docstring 없음
- [218] 스타일 위반: `login_admin` 함수에 docstring 없음
- [224] 스타일 위반: `validate_email` 함수에 docstring 없음

---

### 보안 검사

코드 분석 결과, 총 7가지 보안 취약점을 발견했습니다.

---

## 🔴 취약점 1: 하드코딩된 자격증명 (심각도: 높음)

**위치:** 모듈 최상단 (10~11번 줄) 및 `debug_credentials()` (171~174번 줄)

```python
DB_PASSWORD = "shop_admin_2024"
API_SECRET  = "sk-live-abc123xyz456"
...
def debug_credentials(self):
    secret = "sk-live-abc123xyz456"
    db_pw  = "shop_admin_2024"
    print("DEBUG API KEY:", secret)
    print("DEBUG DB PW:",  db_pw)
```

**문제점:**
- 비밀번호와 API 키가 소스코드에 평문으로 하드코딩되어 있어, GitHub 등 코드 저장소에 업로드 시 즉시 탈취 가능합니다.
- `login_admin()`에서 평문 비밀번호를 직접 비교하므로 비밀번호 노출 시 관리자 권한 탈취가 가능합니다.
- `debug_credentials()`가 운영 환경에서 실행될 경우 모든 자격증명이 로그에 그대로 노출됩니다.
- (OWASP A02:2021 – Cryptographic Failures 해당)

**수정 제안:**
```python
import os
import hashlib

DB_PASSWORD_HASH = os.environ.get("DB_PASSWORD_HASH")
API_SECRET       = os.environ.get("API_SECRET")

def login_admin(password: str) -> bool:
    """관리자 로그인 (해시 비교)"""
    input_hash = hashlib.sha256(password.encode()).hexdigest()
    return input_hash == DB_PASSWORD_HASH
```

---

## 🔴 취약점 2: SQL Injection (심각도: 높음)

**위치:** `orderManager.search_orders()` (88번 줄), `orderManager.search_customers()` (95번 줄)

```python
def search_orders(self, keyword):
    query = "SELECT * FROM orders WHERE customer_name = '" + keyword + "'"
    cur.execute(query)   # Injection 위험

def search_customers(self, keyword):
    query = "SELECT * FROM customers WHERE email = '" + keyword + "'"
    cur.execute(query)   # Injection 위험
```

**문제점:**
- 사용자 입력값 `keyword`를 검증 없이 SQL 쿼리에 직접 연결(concatenation)합니다.
- `' OR '1'='1` 입력 시 전체 데이터 탈취, `'; DROP TABLE orders; --` 입력 시 테이블 삭제가 가능합니다.
- 주문 검색과 고객 검색 두 곳 모두 동일한 패턴으로 취약점이 존재합니다.
- (OWASP A03:2021 – Injection 해당)

**수정 제안:**
```python
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

**위치:** `reportBuilder.build_html_summary()` (158번 줄), `reportBuilder.build_html_customer_list()` (164번 줄)

```python
def build_html_summary(self, title, content):
    html = "<h1>" + title + "</h1>"
    html = html + "<table>" + content + "</table>"
    return html   # html.escape() 처리 없음

def build_html_customer_list(self, customers):
    for c in customers:
        html = html + "<li>" + c["name"] + " (" + c["email"] + ")</li>"
```

**문제점:**
- `title`, `content`, `c["name"]`, `c["email"]` 등 외부 입력값이 이스케이프 처리 없이 HTML에 직접 삽입됩니다.
- `<script>alert('XSS')</script>` 삽입 시 브라우저에서 악성 스크립트가 실행되어 세션 탈취·피싱 등 2차 공격으로 이어질 수 있습니다.
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

## 🔴 취약점 4: Command Injection (심각도: 높음)

**위치:** `orderManager.ping_customer_server()` (120~121번 줄)

```python
def ping_customer_server(self, host):
    result = subprocess.getoutput("ping -c 1 " + host)   # Command Injection
    return result
```

**문제점:**
- 사용자 입력값 `host`가 검증 없이 시스템 명령어에 직접 연결됩니다.
- `; rm -rf /` 또는 `| cat /etc/passwd` 같은 값을 입력하면 서버에서 임의 명령어가 실행됩니다.
- 서버 파일 삭제, 민감 정보 탈취, 악성코드 설치 등 치명적인 피해로 이어질 수 있습니다.
- (OWASP A03:2021 – Injection 해당)

**수정 제안:**
```python
import subprocess
import re

def ping_customer_server(self, host: str) -> str:
    """고객 서버 연결 확인 (Command Injection 방지)"""
    # 허용된 호스트 형식만 허용 (IP 또는 도메인)
    if not re.match(r'^[a-zA-Z0-9.\-]+$', host):
        raise ValueError("유효하지 않은 호스트 형식입니다.")
    result = subprocess.run(
        ["ping", "-c", "1", host],   # 인수 분리로 Injection 방지
        capture_output=True, text=True, timeout=5
    )
    return result.stdout
```

---

## 🔴 취약점 5: Path Traversal (심각도: 높음)

**위치:** `orderManager.export_order_file()` (125~128번 줄)

```python
def export_order_file(self, filename):
    path = "/var/data/orders/" + filename   # Path Traversal
    with open(path, "w") as f:
        f.write(json.dumps(self.orders))
```

**문제점:**
- `filename`에 `../../etc/passwd` 같은 값을 입력하면 의도된 디렉토리를 벗어나 서버의 임의 파일에 접근하거나 덮어쓸 수 있습니다.
- 시스템 설정 파일 변조, 민감 정보 탈취 등 심각한 피해로 이어질 수 있습니다.
- (OWASP A01:2021 – Broken Access Control 해당)

**수정 제안:**
```python
import os

EXPORT_BASE_DIR = "/var/data/orders"

def export_order_file(self, filename: str) -> str:
    """주문 데이터를 파일로 저장 (Path Traversal 방지)"""
    safe_path = os.path.realpath(os.path.join(EXPORT_BASE_DIR, filename))
    if not safe_path.startswith(EXPORT_BASE_DIR):
        raise ValueError("허용되지 않은 파일 경로입니다.")
    with open(safe_path, "w") as f:
        f.write(json.dumps(self.orders))
    return safe_path
```

---

## 🔴 취약점 6: Insecure Deserialization (심각도: 높음)

**위치:** `orderManager.load_orders_from_file()` (131~133번 줄)

```python
def load_orders_from_file(self, filepath):
    with open(filepath, "rb") as f:
        self.orders = pickle.load(f)   # 임의 코드 실행 가능
```

**문제점:**
- `pickle`은 역직렬화 시 파일 내 포함된 임의 Python 코드를 실행할 수 있습니다.
- 공격자가 악성 pickle 파일을 업로드하거나 교체하면 서버에서 임의 코드가 실행됩니다.
- 원격 코드 실행(RCE, Remote Code Execution)으로 이어질 수 있어 매우 위험합니다.
- (OWASP A08:2021 – Software and Data Integrity Failures 해당)

**수정 제안:**
```python
import json

def load_orders_from_file(self, filepath: str) -> None:
    """저장된 주문 데이터 불러오기 (안전한 JSON 사용)"""
    with open(filepath, "r", encoding="utf-8") as f:
        self.orders = json.load(f)   # pickle 대신 json 사용

def save_orders_to_file(self, filepath: str) -> None:
    with open(filepath, "w", encoding="utf-8") as f:
        json.dump(self.orders, f, ensure_ascii=False, indent=2)
```

---

## 🟡 취약점 7: 입력값 검증 미흡 (심각도: 중간)

**위치:** `main()` 함수, choice == "3" 처리 (274번 줄)

```python
oid    = int(input("주문 ID: "))   # 숫자가 아닌 값 입력 시 ValueError
status = input("상태 (pending/paid/shipped/cancelled): ")
# 상태값 허용 목록 검증 없음
```

**문제점:**
- 숫자가 아닌 값 입력 시 `ValueError`가 발생하여 프로그램이 비정상 종료됩니다.
- `status`에 허용 목록 외 임의 문자열이 입력되어도 그대로 저장되어 데이터 무결성이 깨집니다.

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
| 1 | 하드코딩된 자격증명 | 모듈 상단 10~11줄, `debug_credentials` (171줄) | 🔴 높음 |
| 2 | SQL Injection | `search_orders` (88줄), `search_customers` (95줄) | 🔴 높음 |
| 3 | XSS | `build_html_summary` (158줄), `build_html_customer_list` (164줄) | 🔴 높음 |
| 4 | Command Injection | `ping_customer_server` (120줄) | 🔴 높음 |
| 5 | Path Traversal | `export_order_file` (125줄) | 🔴 높음 |
| 6 | Insecure Deserialization | `load_orders_from_file` (131줄) | 🔴 높음 |
| 7 | 입력값 검증 미흡 | `main()` (274줄) | 🟡 중간 |

> 취약점 1~6번은 OWASP Top 10에 포함된 대표적인 보안 취약점으로, 즉시 수정이 필요합니다.

---
