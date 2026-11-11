# 🌰 도토리 미니홈피 – 기말고사 계획표

싸이월드 느낌의 학습 계획표 웹앱입니다.

```
index.html          ← 앱 본체
fonts/              ← 옛날 도스 글꼴 (DOSGothic.woff, DOSIyagiBoldface.woff)
README.md
```

## 기능
- 아이디 + 핀번호(4자리) 로그인 / 가입
- **홈**: 미니룸, 배경음악 플레이어, 오늘 할 일, D-day, 도토리, 친구 새 글을 한 화면에
- 기말고사(11/26)까지 D-day 달력 (매일 자동으로 하루씩 줄어듦)
- 계획표: 할 일 10칸, 시간표, 목표/실제 시간, 메모
- 할 일마다 **사진 인증** → 도토리 🌰 +1 (오늘 날짜만 인증 가능, 인증샷은 나만 봄)
- **다이어리**: 오늘의 일기와 사진 올리기 (서로 친구인 사람에게만 보임)
- 입체(아이소메트릭) 미니룸: 도토리로 가구·벽지·바닥을 사서 드래그로 배치, 캐릭터 말풍선에 오늘 기분 표시
- 다이어리 모양 페이지: 프로필 탭의 "페이지 색 꾸미기"에서 표지색·배경색·포인트색·배경무늬 선택 (친구가 놀러 오면 내 색으로 보임)
- 픽셀 캐릭터 꾸미기 (머리, 앞머리, 눈, 옷 8종, 헤드폰/베레모/머리띠/새싹, 안경, 곰인형 등)
- 쥬크박스: 유튜브 링크 / mp3 주소로 배경음악
- 친구 방문: 방·캐릭터·노래는 누구나 / 다이어리는 서로 추가한 친구만 / 계획표·인증샷은 비공개

## 1. GitHub Pages로 올리기
1. GitHub에서 새 저장소(Repository) 만들기
2. **`index.html`, `README.md`, `fonts` 폴더를 통째로** 업로드 (Add file → Upload files, 폴더를 끌어다 놓기)
3. Settings → Pages → Branch를 `main` / `/ (root)` 로 선택 → Save
4. 1~2분 뒤 `https://내아이디.github.io/저장소이름/` 에서 열려요

`fonts` 폴더가 빠지면 글꼴만 기본 글꼴로 보여요 (앱은 동작).

이것만 해도 동작하지만 **로컬 모드**(그 기기·브라우저에만 저장)예요.
친구 방문과 다른 기기에서 로그인하려면 아래 서버 설정이 필요해요.

## 2. 서버 설정 (Supabase, 무료)
1. https://supabase.com 가입 → New project
2. 왼쪽 SQL Editor에서 아래를 실행 (이미 실행했다면 다시 할 필요 없어요)

```sql
create table docs (
  kind text not null,
  id text not null,
  data jsonb not null,
  updated_at timestamptz default now(),
  primary key (kind, id)
);
alter table docs enable row level security;
create policy "open" on docs for all to anon using (true) with check (true);
```

3. Project Settings → API 에서 **Project URL** 과 **anon public key** 복사
4. `index.html` 맨 위 `CONFIG` 에 붙여넣기

```js
SUPABASE_URL: 'https://xxxx.supabase.co',
SUPABASE_KEY: 'eyJ...'
```

5. 다시 GitHub에 올리면 끝! (친구들이 같은 주소로 접속)

## 3. 바꿀 수 있는 것
- 시험 날짜: `CONFIG.EXAM_DATE`
- 상점 아이템/가격: `ITEMS`, `WALLS`, `FLOORS`
- 기본 색 조합 목록: `THEMES`
- 캐릭터 옷/색: `OUTFITS`, `HAIRS`, `SKINS`

## 글꼴 참고
`fonts/`의 글꼴은 첨부해 주신 DOS 글꼴 파일에서 한글·영문·기호만 남겨 용량을 줄인 것이에요.
공개 저장소에 올리면 누구나 내려받을 수 있으니, **글꼴 배포처의 라이선스(재배포 허용 여부)를 한 번 확인**해 주세요.

## ⚠️ 보안 참고
- 이 앱은 가벼운 학습용이에요. 서버 규칙이 "누구나 읽기/쓰기"라서, 기술에 밝은 사람이 직접 요청을 보내면 남의 계획표·일기·사진도 볼 수 있어요. "비공개"는 화면에서만 지켜져요.
- 핀번호는 암호화(해시)해서 저장하지만 4자리라 뚫리기 쉬워요. **중요한 비밀번호를 핀번호로 쓰지 말라고** 학생들에게 알려 주세요.
- 사진은 640px로 줄여서 저장돼요. 얼굴·교복·주소 등 개인정보가 나오는 사진은 올리지 않기!
- 더 안전하게 하려면 Supabase Auth + 사용자별 RLS 규칙으로 바꾸는 방법이 있어요.
