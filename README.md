# 🐾 경수오빠못해조 (Pet Commerce Platform)

![Python](https://img.shields.io/badge/Python-3.11-blue)
![FastAPI](https://img.shields.io/badge/FastAPI-Backend-green)
![MySQL](https://img.shields.io/badge/MySQL-Database-orange)
![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4.1--mini-black)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-ML-yellow)

반려동물을 위한 **온라인 쇼핑몰 + 커뮤니티 플랫폼**

상품 구매부터 반려인 커뮤니티, AI 챗봇 고객센터,  
그리고 머신러닝 기반 상품 추천 기능까지 제공하는 웹 서비스입니다.

---

# 👋 프로젝트 소개 (Overview)

**경수오빠못해조**는 반려동물(강아지, 고양이, 새, 햄스터 등)을 위한  
용품을 판매하고 반려인들이 소통할 수 있는 **Pet Commerce 플랫폼**입니다.

🛒 **쇼핑몰**
- 카테고리별 상품 조회
- 장바구니
- 재고 관리
- 주문 및 결제 시스템

📝 **커뮤니티**
- 사용자 간 자유로운 소통을 위한 게시판
- 게시글 CRUD
- 작성자 권한 관리

🤖 **AI 챗봇**
- OpenAI **GPT-4.1-mini 기반**
- 고객센터 자동 응답 챗봇

🔮 **상품 추천**
- 사용자 정보(성별 / 연령대) 기반 머신러닝 추천
- Random Forest 기반 추천 시스템

🎨 **반응형 UI**
- Glassmorphism 디자인
- 반응형 웹 인터페이스

---

# 🛠 Tech Stack

| 구분 | 기술 | 설명 |
|-----|------|------|
| Backend | FastAPI | 고성능 비동기 웹 프레임워크 |
| Database | MySQL | 관계형 데이터베이스 |
| ORM | SQLAlchemy | Python 객체와 DB 매핑 |
| AI / ML | OpenAI GPT, Scikit-learn | CS 챗봇 및 상품 추천 알고리즘 |
| Frontend | HTML, CSS, Jinja2 | 반응형 UI 및 템플릿 엔진 |
| Infrastructure | AWS | 클라우드 서버 및 스토리지 |

---

# 🏗 System Architecture

```
User
 │
 ▼
Frontend (HTML / Jinja2)
 │
 ▼
FastAPI Backend
 │
 ├── Authentication
 ├── Shopping Cart
 ├── Orders
 ├── Community Board
 ├── AI Chatbot
 │
 ▼
Database (MySQL)
 │
 ├── Users
 ├── Products
 ├── Orders
 ├── Carts
 ├── Reviews
 └── Memo
 │
 ▼
AI / ML Services
 ├── GPT Chatbot
 └── RandomForest Recommendation
```

---

# 📂 Directory Structure

프로젝트는 **MVC (Model-View-Controller)** 패턴을 기반으로 기능별로 구조화되어 있습니다.

```
project1
│
├── main.py                  # [Entry] 앱 실행 및 설정 진입점
├── database.py              # DB 연결 엔진 설정
├── dependencies.py          # DB 세션 관리 및 인증 의존성
├── schemas.py               # Pydantic 데이터 검증 모델
│
├── routers/                 # [Controller] URL 라우팅 및 비즈니스 로직
│   ├── auth.py              # 회원가입 / 로그인 / 정보수정
│   ├── cart.py              # 장바구니 로직
│   ├── orders.py            # 주문 및 결제 처리
│   ├── memos.py             # 게시판 CRUD
│   └── chatbot.py           # AI 챗봇 API
│
├── data/                    # [Model / DAO] DB 쿼리 모듈
│   ├── products.py          # 상품 및 재고 관리 쿼리
│   ├── orders.py            # 주문 트랜잭션 쿼리
│   └── ...                  # auth, cart, memos 관련 데이터 처리
│
└── templates/               # [View] 사용자 인터페이스 (HTML)
    ├── main.html            # 메인 페이지
    ├── product_detail.html  # 상품 상세 및 리뷰
    ├── mypage.html          # 마이페이지
```

---

# 🚀 핵심 기능 및 로직 (Key Features)

## 🛒 쇼핑 및 주문 (Shopping & Order)

### 재고 관리
주문 시 `product_data`를 통해 **실시간 재고를 확인**합니다.

### 트랜잭션 처리

`create_order()` 함수에서 다음 작업이 **하나의 트랜잭션**으로 처리됩니다.

- 결제 금액 계산
- 주문 생성
- 주문 상세 품목 기록
- 재고 차감

### 장바구니

세션 기반이 아닌 **DB 기반 장바구니 (carts 테이블)** 사용

- 로그아웃 후에도 장바구니 유지
- 사용자별 장바구니 관리

---

## 📝 커뮤니티 게시판 (Community Board)

### 접근 제어

세션 검증을 통해 **로그인한 사용자만 게시글 작성 / 수정 / 삭제 가능**

### CRUD 구현

게시글 수정 및 삭제 시

- 작성자 `user_id` 확인
- 본인 글만 수정/삭제 가능

### 편의 기능

- **내 글만 보기 필터**
- **제목 / 내용 / 작성자 검색 기능**

---

## 🤖 AI 챗봇 (AI Chatbot)

OpenAI **GPT-4.1-mini 기반 고객센터 챗봇**

### Role Playing

챗봇에게 **"쇼핑몰 고객센터 직원" 페르소나** 부여

### Context Injection

`faq.json` 데이터를 프롬프트에 삽입하여

- 배송 정책
- 환불 정책
- 주문 관련 문의

등에 대해 **쇼핑몰 정책에 맞는 답변 생성**

---

## 🔮 상품 추천 시스템 (Recommendation)

### Machine Learning

Scikit-learn의 **RandomForestClassifier** 사용

### 입력 데이터

- 성별
- 연령대

### 출력

선호 확률이 높은 **상품 Top 10 추천**

### 자동화

`schedule` 라이브러리를 통해  
추천 로직 (`batch.py`)을 **주기적으로 실행**

---

# 💾 데이터베이스 설계 (ERD Summary)

| Table | Description | Relation |
|------|-------------|---------|
| Users | 회원 정보 | - |
| Products | 상품 및 재고 | Categories와 N:1 |
| Orders | 주문 내역 | Users와 N:1 |
| Order_Items | 주문 상세 품목 | Orders, Products 연결 |
| Carts | 사용자 장바구니 | Users와 1:1 |
| Memo | 게시판 게시글 | Users와 N:1 |
| Reviews | 상품 리뷰 | Products, Users와 N:1 |

---

# 🔌 API Structure

| API | Description |
|----|-------------|
| `/auth` | 회원가입 / 로그인 |
| `/cart` | 장바구니 |
| `/orders` | 주문 |
| `/memos` | 게시판 |
| `/chatbot` | AI 챗봇 |

---

# 🌐 Deployment

프로젝트는 **Render 클라우드 플랫폼**을 사용하여 실제 서비스 환경에 배포되었습니다.

🔗 **Service URL**

https://project1-c2xk.onrender.com/

현재는 서버가 비활성 상태이지만  
배포 테스트 및 서비스 동작 검증까지 완료되었습니다.

### Deployment Stack

| Component | Technology |
|-----------|------------|
| Cloud Platform | Render |
| Backend | FastAPI |
| Database | MySQL |
| Environment | Python 3.11 |


---

# ⚙️ Installation

```bash
git clone https://github.com/your-repo/project1.git

cd project1
pip install -r requirements.txt
```

---

### Run Server

```
uvicorn main:app --reload
```

---

# 📸 Screenshots

## 메인 페이지

<img width="600" alt="project1main2" src="https://github.com/user-attachments/assets/fef325f5-5366-4136-a1d4-c459885419ed" />


## 상품 상세

(이미지 추가 예정)

## 커뮤니티

(이미지 추가 예정)

## AI 챗봇
<img width="333" height="547" alt="project1chat" src="https://github.com/user-attachments/assets/72916057-e5e7-4b01-90bb-45e1ce2abdab" />


## 데이터 베이스 ERD 
<img width="500" alt="project1db" src="https://github.com/user-attachments/assets/42684bd1-e34c-40a3-95af-854dff9bc0bd" />

---

# 🎯 Project Goals

- 반려동물 쇼핑 경험 개선
- AI 기반 고객 응대 자동화
- 머신러닝 기반 상품 추천 제공
- 반려인 커뮤니티 플랫폼 구축
