# 1004보금자리 홍보 슬라이드쇼

사진이 풀스크린으로 나오고, 제목/설명이 계속 바뀌는 히어로 슬라이드쇼 웹페이지입니다.
브라우저에서 바로 사진과 문구를 편집할 수 있는 편집 화면이 내장되어 있고,
편집한 내용을 내보내서 `data/content.json`에 반영하면 GitHub Pages를 통해
모든 방문자에게 그대로 보여집니다.

## 구성

- `index.html` : 슬라이드쇼 재생 화면 + 편집 화면이 모두 들어있는 단일 페이지
- `data/content.json` : 실제로 배포되는 슬라이드/문구 내용 (사진, 제목, 순서 등)
- `.github/workflows/pages.yml` : 기본 브랜치(현재 `claude/determined-meitner-vwgjgh`)에 push하면 자동으로 GitHub Pages에 배포

빌드 과정이 없는 순수 정적 페이지라서, 별도 설치 없이 파일을 그대로 열거나
GitHub Pages에 올리면 바로 동작합니다.

## 동작 방식 (중요)

- 페이지를 열면 `data/content.json`을 먼저 읽어와서 **처음 방문하는 브라우저의 기본값**으로 사용합니다.
- 화면 오른쪽 위 **"✎ 사진 편집"** 버튼을 누르면 편집 화면이 열립니다. 여기서 고친 내용은
  일단 **지금 사용 중인 브라우저에만** 자동 저장됩니다. (localStorage)
- 즉, 편집 화면에서 사진/문구를 바꾸기만 하면 나만 보이고, 다른 사람이나 실제 GitHub Pages
  주소로 접속하는 방문자에게는 반영되지 않습니다.

## 사진/문구를 추가해서 실제로 GitHub에 반영하는 방법

### 방법 A. 편집 화면에서 "GitHub에 저장" 버튼으로 바로 반영 (추천, git 명령 몰라도 됨)

1. **"✎ 사진 편집"** → **"🚀 GitHub에 바로 저장하기"** 칸에 아래 정보를 한 번만 입력합니다.
   - 저장소: `bokjin1004-sys/1004`
   - 브랜치: 저장소의 기본 브랜치 이름 (지금은 `claude/determined-meitner-vwgjgh`)
   - Personal Access Token: GitHub → 프로필 → Settings → Developer settings →
     Personal access tokens → Fine-grained tokens → Generate new token에서 발급.
     **Repository access**는 이 저장소 하나만 선택하고, **Permissions → Contents**를
     **Read and write**로만 설정해서 만드세요. (다른 권한은 켤 필요 없습니다.)
   - 이 정보는 이 브라우저에만 저장되고, GitHub 서버로만 전송됩니다.
2. 사진·제목·순서를 편집합니다.
3. **"🚀 GitHub에 저장"** 버튼을 누르면 즉시 `data/content.json`에 커밋되고,
   1분 정도 뒤 GitHub Pages 배포가 끝나면 모든 방문자에게 반영됩니다.
   (저장소의 **Actions** 탭에서 배포 진행 상황을 확인할 수 있습니다.)

### 방법 B. 직접 파일을 내려받아 git push (git 사용에 익숙한 경우)

1. **"✎ 사진 편집"**에서 사진을 추가/교체하고 제목·설명·순서를 편집합니다.
   - 사진은 "파일 선택"으로 올리면 자동으로 파일 안에 그대로 포함되고(별도 이미지 파일 필요 없음),
     인터넷 상의 이미지라면 "이미지 주소(URL)"만 입력해도 됩니다.
2. **"내보내기 (백업 파일)"**를 클릭해 `content.json` 파일을 내려받습니다.
3. 내려받은 `content.json`으로 저장소의 `data/content.json` 파일을 덮어씁니다.
4. 변경 사항을 커밋하고 push합니다.

   ```bash
   git add data/content.json
   git commit -m "슬라이드 내용 업데이트"
   git push
   ```

5. 기본 브랜치에 push되면 GitHub Actions가 자동으로 GitHub Pages에 다시 배포합니다.

> 편집 화면의 "가져오기 (백업 파일)" 버튼으로 `content.json`을 다시 불러와서
> 이어서 편집할 수도 있습니다.

## GitHub Pages 처음 설정하기

1. GitHub 저장소의 **Settings → Pages**로 이동합니다.
2. **Source**를 `GitHub Actions`로 선택합니다. (한 번만 설정하면 됩니다)
3. 기본 브랜치에 push하면 `https://<사용자명>.github.io/<저장소명>/` 주소로 자동 배포됩니다.

## 그 밖의 기능

- 화면 하단의 재생/일시정지 버튼, "다음" 버튼으로 슬라이드를 직접 넘겨볼 수 있습니다.
- 화면을 클릭해도 재생/일시정지가 토글됩니다.
- 편집 화면의 **"🎬 영상 다운로드"** 버튼으로 슬라이드쇼를 mp4/webm 영상 파일로 내려받을 수 있습니다.
  (최신 Chrome 브라우저 권장. 외부 URL 사진 중 일부는 브라우저 보안 정책상 영상에 포함되지
  않을 수 있으니, 영상으로 내보낼 사진은 "파일 선택"으로 직접 올리는 것을 권장합니다.)

## 로컬에서 미리보기

`data/content.json`을 `fetch`로 불러오기 때문에, 파일을 더블클릭해서 여는 방식은
브라우저 보안 정책 때문에 정상적으로 동작하지 않습니다. 반드시 간단한 로컬 서버를 통해 열어주세요.

```bash
python3 -m http.server 8000
```

그 다음 브라우저에서 `http://localhost:8000` 접속.
