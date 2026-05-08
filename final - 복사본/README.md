# 개인 홈페이지 — 게시판 Firebase 설정 가이드

게시판 기능은 Firebase Firestore를 사용합니다.
아래 순서대로 따라하면 5~10분 안에 설정 완료됩니다.

---

## 1단계 — Firebase 프로젝트 만들기

1. https://console.firebase.google.com 접속 (구글 계정 로그인)
2. **"프로젝트 추가"** 클릭
3. 프로젝트 이름 입력 (예: `my-homepage`)
4. Google 애널리틱스는 **사용 안 함** 선택 후 **프로젝트 만들기**

---

## 2단계 — 웹 앱 등록 및 config 복사

1. 프로젝트 홈에서 **`</>`** (웹) 아이콘 클릭
2. 앱 닉네임 입력 후 **"앱 등록"** 클릭
3. 아래처럼 생긴 `firebaseConfig` 코드가 나타납니다

```js
const firebaseConfig = {
  apiKey: "AIza...",
  authDomain: "my-homepage.firebaseapp.com",
  projectId: "my-homepage",
  storageBucket: "my-homepage.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123...:web:abc..."
};
```

4. 이 값을 복사해서 `js/firebase-config.js` 파일의 해당 위치에 붙여넣기

---

## 3단계 — Firestore 데이터베이스 만들기

1. 왼쪽 메뉴 **"Firestore Database"** 클릭
2. **"데이터베이스 만들기"** 클릭
3. **테스트 모드로 시작** 선택 (누구나 읽기/쓰기 가능 — 개발 단계용)
4. 위치는 **asia-northeast3 (서울)** 선택 후 완료

> ⚠️ 테스트 모드는 30일 후 자동으로 읽기/쓰기가 차단됩니다.
> 30일 이후에도 사용하려면 Firestore 규칙을 아래처럼 변경하세요.

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /posts/{postId} {
      allow read, write: if true;
    }
  }
}
```

---

## 4단계 — firebase-config.js 수정

`js/firebase-config.js` 파일을 열어서 아래처럼 수정합니다.

```js
const firebaseConfig = {
  apiKey:            "복사한-값",
  authDomain:        "복사한-값",
  projectId:         "복사한-값",
  storageBucket:     "복사한-값",
  messagingSenderId: "복사한-값",
  appId:             "복사한-값"
};
```

---

## 5단계 — 실행 확인

VS Code에서 Live Server로 열고 Board 섹션에서 글쓰기를 해보세요.
Firebase 콘솔 Firestore에 데이터가 쌓이면 성공입니다!

---

## 파일 구조

```
my-homepage/
├── index.html
├── css/
│   ├── style.css       ← 기존 스타일
│   └── board.css       ← 게시판 스타일 (새로 추가)
├── js/
│   ├── firebase-config.js  ← Firebase 설정 (본인 값으로 교체 필요!)
│   ├── board.js            ← 게시판 CRUD 로직 (새로 추가)
│   └── main.js             ← 기존 인터랙션
└── README.md
```

---

## index.html 수정 사항 요약

- 네비게이션에 **Board** 링크 추가
- `</body>` 위에 Firebase SDK 및 새 스크립트 로드 추가
- Contact 섹션 번호 04 → **06** 으로 변경 (Board가 05)
- `<link rel="stylesheet" href="css/board.css" />` 추가
