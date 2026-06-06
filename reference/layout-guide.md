# Jonghwan Lee 홈페이지 — 콘텐츠 등록 지침서

> 레이아웃은 완성된 상태. 이 지침서는 새로운 전시, 작업, 글을 등록할 때 참고용.

---

## 사이트 구조

```
jonghwanlee-No2/
├── index.html          ← 메인 (전시 그리드 + 갤러리)
├── works.html          ← 작업 시리즈 목록 + 갤러리
├── cv.html             ← 이력
├── note.html           ← 텍스트/노트 목록 + 리더
├── contact.html        ← 연락처
├── styles.css          ← (미사용, 각 페이지 내부 style)
├── *.pdf / *.html      ← 노트 콘텐츠 파일
└── [이미지 폴더들]/    ← 전시별 사진 폴더
```

**GitHub**: https://github.com/Jonghwaneddie/jonghwanlee-No2
**사이트**: https://jonghwaneddie.github.io/jonghwanlee-No2/

---

## 공통 스타일

- **폰트**: Raleway (Google Fonts), weight 200/300/400
- **헤더**: 고정, 전체 폭, 좌측 이름 + 우측 nav (Notes / CV / Works / Contact)
- **모바일** (≤768px): nav 숨기고 햄버거 메뉴
- **콘텐츠 영역**: `max-width: 1050px`, `margin: 0 auto`, 좌측정렬
- **패딩**: `padding: 75px 60px 60px` (모바일: `70px 25px 40px`)

---

## 1. 새 전시 등록 (index.html)

### 1-1. 사진 폴더 준비

```
homepage/[폴더명]/
  img1.jpg
  img2.jpg
  ...
```
- 폴더명 예: `2026newtitle/`
- 같은 폴더를 루트에도 복사 (GitHub Pages용)

### 1-2. exhibitions 배열에 추가

`<script>` 안의 `const exhibitions = [...]`에 항목 추가:

**Solo 전시:**
```javascript
{
    id: '전시ID', type: 'solo', title: '전시 제목', year: '2026',
    info: '날짜<br>장소 주소',
    description: '설명 텍스트 (HTML 가능, <br> 줄바꿈, <span class="fade-text"> 연한 글씨)',
    imageCount: 10  // 실제 이미지 수 (allImageData에 등록할 개수)
},
```

**Group 전시:**
```javascript
{
    id: '전시ID', type: 'group', title: '전시 제목', year: '2026',
    info: '날짜<br>장소 주소',
    artists: '<span class="fade-text">Participating Artists: 이름1, 이름2, ...</span>',
    imageCount: 5
},
```

- Solo는 `// --- Solo ---` 아래에, Group은 `// --- Group ---` 아래에 추가
- 최신 전시가 위쪽

### 1-3. allImageData 배열에 이미지 등록

```javascript
{ id: '고유ID', projectId: '전시ID(exhibitions의 id와 동일)', isDefault: false,
  folder: '폴더명', fileName: '파일명.jpg',
  description: '작품명, 연도, 재료, 크기',
  url: '폴더명/파일명.jpg',
  isGroupMain: true },
```

- `projectId`: exhibitions 배열의 `id`와 반드시 일치
- `isDefault: false` (true는 INDEX_DEFAULT 캐러셀 전용)
- `isGroupMain: true`: 단독 표시 이미지
- 그룹 이미지 (캐러셀): 같은 `groupId`를 공유, 첫 번째에만 `isGroupMain: true`

```javascript
// 그룹 예시 (설치 전경 여러 장)
{ id: 'new_1', projectId: 'NewShow', ..., description: 'Installation View',
  url: 'newfolder/1.jpg', isGroupMain: true, groupId: 'new_Group_A' },
{ id: 'new_2', projectId: 'NewShow', ..., description: '',
  url: 'newfolder/2.jpg', groupId: 'new_Group_A' },
```

### 1-4. 갤러리 동작

- 전시 클릭 → 첫 번째 사진 위에 **반투명 오버레이**로 전시 정보 표시
- 다음(→) 누르면 오버레이만 사라지고 같은 첫 사진 유지
- 이후 →/← 로 사진 탐색
- ESC 또는 ← Back으로 복귀

### 1-5. 썸네일

- 그리드에 표시되는 썸네일은 `allImageData`에서 해당 전시의 **첫 번째 이미지**가 자동 선택됨
- `aspect-ratio: 4/3`, `object-fit: cover`
- 호버 시 blur 오버레이 + 제목/연도

---

## 2. 새 작업 시리즈 등록 (works.html)

### 2-1. work-list에 항목 추가

```html
<div class="work-item" data-project-id="작업시리즈ID">
    <span class="work-title">시리즈 제목</span>
    <span class="work-year">연도</span>
</div>
```
- 최신이 위쪽

### 2-2. allImageData 배열에 이미지 등록

index.html과 동일한 형식. `projectId`가 `data-project-id`와 일치해야 함.

### 2-3. workDescriptions에 설명 추가 (선택)

```javascript
const workDescriptions = {
    '작업시리즈ID': '설명 텍스트 (HTML 가능)',
    ...
};
```
- 설명이 있으면 갤러리 오픈 시 첫 이미지 위에 반투명 오버레이로 표시
- 없으면 바로 이미지부터 시작

### 2-4. 갤러리 동작

index.html과 동일한 오버레이 방식.

---

## 3. 새 글/노트 등록 (note.html)

### 3-1. 콘텐츠 파일 준비

- **PDF**: `homepage/파일명.pdf` → 루트에도 복사
- **HTML**: `homepage/파일명.html` → 루트에도 복사

### 3-2. note-list에 항목 추가

```html
<div class="note-item" data-note-id="파일명(확장자 제외)">
    <span class="note-title">제목, 저자</span>
    <span class="note-year">연도</span>
</div>
```
- Texts 섹션 또는 Notes 섹션 아래에 추가
- 최신이 위쪽

### 3-3. PDF인 경우 pdfProjects 배열에 추가

```javascript
const pdfProjects = ['loggiatext', 'asdf', 'loggianote', '새파일명'];
```
- 여기 없으면 `.html` 파일로 fetch 시도

### 3-4. 리더 동작

- 클릭 → 전체화면 리더 (PDF: iframe / HTML: fetch 후 렌더링)
- ← Back 또는 ESC로 복귀

---

## 4. CV 업데이트 (cv.html)

### 전시 추가

```html
<div class="cv-item">
    <span class="cv-title">전시 제목</span>
    <span class="cv-venue">연도, 장소, 도시, 국가</span>
</div>
```
- Solo 또는 Group 섹션 아래에 추가
- 최신이 위쪽

### 학력 추가

`.edu` div 안에 `<br>`로 줄 추가.

---

## 5. Contact 업데이트 (contact.html)

- 중앙정렬 (`text-align: center`)
- 이메일, SNS 링크 등 `.contact-item` div 추가

---

## 배포 순서

```bash
# 1. homepage/ 폴더에서 작업
# 2. 이미지 폴더가 있으면 루트에도 복사
cp -R homepage/새폴더/ ./새폴더/

# 3. HTML 파일 루트에 복사
cp homepage/index.html ./index.html
cp homepage/works.html ./works.html
# (수정한 파일만)

# 4. git add & commit & push
git add .
git commit -m "설명"
git push origin main
```

- GitHub Pages 배포: push 후 1~2분 소요
- 사이트 확인: https://jonghwaneddie.github.io/jonghwanlee-No2/

---

## 이미지 폴더 매핑 (현재 등록된 전시)

| 전시 | 폴더 |
|------|------|
| Expanding Horizons | `hk2026/` |
| grid 5 | `jp2026/` |
| Loggia | `loggiaimages/`, `2025loggia1/`, `2025loggia2/` |
| Surgical Room | `2025surgicalroom/` |
| CYLINDER x South Parade | `cxs/` |
| Hypercube | `hypercubeimages/`, `2023hypercube/` |
| The Good Neighbor | `good/` |
| Cabinet | `cabinetimages/` |
| The Wild Bunch | `2022TWR/` |
| Fonds | `2022fonds/` |
| Room of Convexity and Concavity | `lala/`, `lala2/` |
| TORQUE 1 / GEAR SHIFT | `cylinder/` |

---

## 주의사항

- `id`와 `projectId`는 반드시 일치해야 갤러리가 작동함
- 이미지 URL은 루트 기준 상대경로 (`폴더명/파일명.jpg`)
- `homepage/`에서 작업 후 반드시 루트에 복사해야 GitHub Pages에 반영
- 커밋 전 로컬에서 `open index.html`로 확인 권장
