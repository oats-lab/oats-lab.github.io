---
title: "TJPUB - README.md"
date: 2026-06-22 00:00:00 +0900
categories: [Python]
tags: [trilium-jekyll-publisher, python, trilium, jekyll, githubpages]
mermaid: true
---

<h1><strong>Trilium Jekyll Publisher</strong></h1><p>Trilium Jekyll Publisher(<code>tjpub</code>)는 Trilium Notes에서 작성한 블로그 초안을 GitHub Pages + Jekyll Chirpy 블로그 포스트로 변환하고 발행하는 Python CLI 도구입니다.</p><p>Trilium은 개인 지식 저장소와 초안 작성 공간으로 유지하고, GitHub Pages 저장소에는 공개 가능한 최종 포스트와 이미지 파일만 반영하는 것을 목표로 합니다.</p><hr><h2><strong>1. 프로젝트 소개</strong></h2><p>이 프로젝트의 목적은 Trilium에서 작성한 Markdown 기반 글을 Jekyll/Chirpy 규칙에 맞는 포스트 파일로 안정적으로 변환하는 것입니다.</p><p>핵심 방향은 다음과 같습니다.</p><ul><li>Trilium은 비공개 초안, 개인 메모, 작성 공간으로 사용한다.</li><li>GitHub Pages는 공개 블로그와 최종 포스트 저장소로 사용한다.</li><li>Publisher는 Trilium 초안을 Jekyll 포스트로 변환하고, 이미지와 Front Matter를 정리한다.</li><li>민감 정보, API 토큰, 개인 메모 전체는 GitHub Pages 저장소에 커밋하지 않는다.</li></ul><pre><code class="language-text-x-trilium-auto">Trilium
  -&gt; 블로그 초안 / 개인 지식 정리
  -&gt; Markdown 본문 / 이미지 / 메타데이터

tjpub
  -&gt; Front Matter 생성
  -&gt; 이미지 복사 및 링크 변환
  -&gt; 발행 전 검증
  -&gt; 선택적 Git commit/push

GitHub Pages + Jekyll Chirpy
  -&gt; 공개 블로그
</code></pre><hr><h2><strong>2. 전체 동작 흐름</strong></h2><h3><strong>로컬 Markdown 파일 기반</strong></h3><pre><code class="language-text-x-trilium-auto">Trilium에서 Markdown export
  -&gt; tjpub from-file 실행
  -&gt; Front Matter 생성
  -&gt; 이미지 파일을 assets/img/posts/&lt;slug&gt;/로 복사
  -&gt; _posts/YYYY-MM-DD-slug.md 생성
  -&gt; 선택적으로 Git commit/push
</code></pre><h3><strong>Trilium noteId 기반</strong></h3><pre><code class="language-text-x-trilium-auto">Trilium noteId 입력
  -&gt; ETAPI로 노트 제목/본문/속성 조회
  -&gt; #slug, #category, #tag 등 메타데이터 파싱
  -&gt; 첨부 이미지 다운로드 및 링크 변환
  -&gt; Jekyll 포스트 생성
  -&gt; 선택적으로 Git commit/push
</code></pre><hr><h2><strong>3. 주요 기능</strong></h2><p>현재 구현된 기능:</p><ul><li><code>tjpub from-file</code>: 로컬 Markdown 파일을 Jekyll Chirpy 포스트로 변환</li><li><code>tjpub from-note</code>: Trilium noteId로 노트를 가져와 포스트로 변환</li><li>YAML Front Matter 생성</li><li><code>YYYY-MM-DD-slug.md</code> 파일명 생성</li><li>Jekyll 저장소의 <code>_posts/</code>에 포스트 생성</li><li>로컬 Markdown 이미지 링크 변환</li><li>Trilium HTML <code>&lt;img src="..."&gt;</code> 이미지 링크 변환</li><li>Trilium ETAPI 첨부 이미지 다운로드</li><li>이미지 저장 경로: <code>assets/img/posts/&lt;slug&gt;/</code></li><li><code>title</code>, <code>date</code>, <code>slug</code>, <code>categories</code>, <code>tags</code> 검증</li><li>Trilium 내부 링크 및 민감 문자열 경고</li><li><code>--dry-run</code> 미리보기</li><li><code>--overwrite</code> 덮어쓰기</li><li><code>--strict</code> 경고를 에러로 처리</li><li><code>--git</code>, <code>--push</code>, <code>--commit-message</code> Git 자동화</li><li>Trilium 메타데이터 규칙 파싱</li></ul><p>예정 기능:</p><ul><li>Trilium 내부 note 링크를 블로그 URL로 변환</li><li><code>#blog-ready</code> 필수 발행 조건 옵션</li><li><code>#date=YYYY-MM-DD</code> 메타데이터 지원</li><li>여러 노트 일괄 발행</li><li>README/CLI 예제 자동 동기화</li><li>GUI 또는 Trilium Script Button 연동</li></ul><hr><h2><strong>4. 설치 방법</strong></h2><p>Python 3.10 이상을 권장합니다.</p><pre><code class="language-text-x-trilium-auto">python -m venv .venv
</code></pre><p>PowerShell:</p><pre><code class="language-text-x-trilium-auto">.\.venv\Scripts\Activate.ps1
</code></pre><p>패키지 설치:</p><pre><code class="language-text-x-trilium-auto">python -m pip install -e .
</code></pre><p>개발/테스트 의존성까지 설치:</p><pre><code class="language-text-x-trilium-auto">python -m pip install -e .[dev]
</code></pre><p>설치 확인:</p><pre><code class="language-text-x-trilium-auto">tjpub --help
tjpub version
</code></pre><hr><h2><strong>5. 설정 방법</strong></h2><h3><strong>설정 파일</strong></h3><p><code>config.example.yml</code>을 복사해서 <code>config.yml</code>을 만듭니다.</p><pre><code class="language-text-x-trilium-auto">Copy-Item config.example.yml config.yml
</code></pre><p>예시:</p><pre><code class="language-text-x-trilium-auto">trilium:
  base_url: "http://localhost:8080"
  token_env: "TRILIUM_ETAPI_TOKEN"
  export_dir: "./exports/trilium"

jekyll:
  site_dir: "../oats-lab.github.io"
  posts_dir: "_posts"
  images_dir: "assets/img/posts"
  timezone: "+0900"

publisher:
  default_layout: "post"
  default_categories:
    - Blog
  default_tags: []
  dry_run: true
</code></pre><p><code>config.yml</code>은 실제 사용자 설정 파일입니다. Git에 커밋하지 마세요.</p><p>설정 확인:</p><pre><code class="language-text-x-trilium-auto">tjpub check-config
</code></pre><p>다른 설정 파일을 지정하려면:</p><pre><code class="language-text-x-trilium-auto">tjpub check-config --config config.example.yml
</code></pre><h3><strong>환경 변수</strong></h3><p>Trilium ETAPI 토큰은 설정 파일에 직접 저장하지 않고 환경 변수로 관리합니다.</p><p>PowerShell 임시 설정:</p><pre><code class="language-text-x-trilium-auto">$env:TRILIUM_ETAPI_TOKEN="여기에_ETAPI_토큰"
</code></pre><p><code>.env</code> 파일을 사용할 수도 있지만, 현재 CLI가 <code>.env</code>를 자동 로드하지는 않습니다. <code>.env</code>를 사용한다면 별도 쉘 도구나 추후 기능으로 로드해야 합니다.</p><p><code>.env</code> 예시:</p><pre><code class="language-text-x-trilium-auto">TRILIUM_ETAPI_TOKEN=여기에_ETAPI_토큰
</code></pre><p>반드시 <code>.env</code>와 <code>config.yml</code>은 <code>.gitignore</code>에 포함해야 합니다.</p><hr><h2><strong>6. 사용 방법</strong></h2><h3><strong>Markdown 파일에서 발행</strong></h3><pre><code class="language-text-x-trilium-auto">tjpub from-file ".\exports\sample.md" `
  --title "Proxmox VE 설치하기" `
  --date 2026-06-15 `
  --slug proxmox-install `
  --category Homelab `
  --category Proxmox `
  --tag proxmox `
  --tag pve `
  --tag virtualization
</code></pre><p>생성 결과:</p><pre><code class="language-text-x-trilium-auto">../oats-lab.github.io/_posts/2026-06-15-proxmox-install.md
</code></pre><p>생성되는 Front Matter 예시:</p><pre><code class="language-text-x-trilium-auto">---
title: "Proxmox VE 설치하기"
date: 2026-06-15 00:00:00 +0900
categories: [Homelab, Proxmox]
tags: [proxmox, pve, virtualization]
---
</code></pre><h3><strong>dry-run</strong></h3><p>파일을 만들지 않고 결과 경로, 검증 결과, 이미지 처리 예정 목록만 확인합니다.</p><pre><code class="language-text-x-trilium-auto">tjpub from-file ".\exports\sample.md" `
  --title "Proxmox VE 설치하기" `
  --date 2026-06-15 `
  --slug proxmox-install `
  --category Homelab `
  --tag proxmox `
  --dry-run
</code></pre><p>Trilium noteId 기반 dry-run:</p><pre><code class="language-text-x-trilium-auto">tjpub from-note dg4SLvEhyiJK --dry-run
</code></pre><h3><strong>overwrite</strong></h3><p>같은 날짜와 slug의 포스트가 이미 있으면 기본적으로 실패합니다.</p><pre><code class="language-text-x-trilium-auto">Output file already exists
</code></pre><p>덮어쓰려면 <code>--overwrite</code>를 사용합니다.</p><pre><code class="language-text-x-trilium-auto">tjpub from-note dg4SLvEhyiJK --overwrite
</code></pre><h3><strong>이미지 포함 글 발행</strong></h3><p>로컬 Markdown export 예:</p><pre><code class="language-text-x-trilium-auto">![설치 화면](image.png)
![네트워크 설정](images/network.png)
</code></pre><p>Trilium HTML 이미지 export 예:</p><pre><code class="language-text-x-trilium-auto">&lt;img src="image.webp"&gt;
</code></pre><p>Trilium ETAPI 첨부 이미지 예:</p><pre><code class="language-text-x-trilium-auto">&lt;img src="api/attachments/Ua66tn0Wqqib/image/profile.png"&gt;
</code></pre><p><code>tjpub</code>는 이미지를 다음 경로로 정리합니다.</p><pre><code class="language-text-x-trilium-auto">assets/img/posts/&lt;slug&gt;/
</code></pre><p>본문 링크는 다음처럼 바뀝니다.</p><pre><code class="language-text-x-trilium-auto">![설치 화면](/assets/img/posts/proxmox-install/image.png)
</code></pre><p>HTML 이미지에 <code>alt</code>가 없으면 배포 검증 실패를 줄이기 위해 <code>alt=""</code>를 자동으로 추가합니다.</p><p>이미지가 누락된 경우 기본은 에러입니다. 경고만 출력하고 진행하려면:</p><pre><code class="language-text-x-trilium-auto">tjpub from-file ".\exports\sample.md" `
  --title "이미지 테스트" `
  --date 2026-06-15 `
  --slug image-test `
  --category Blog `
  --tag image `
  --allow-missing-images
</code></pre><h3><strong>Git commit/push 옵션</strong></h3><p>기본 동작은 파일 생성까지만 수행합니다. Git 자동화는 명시적으로 <code>--git</code>을 줄 때만 실행됩니다.</p><pre><code class="language-text-x-trilium-auto">tjpub from-note dg4SLvEhyiJK --git
</code></pre><p>commit 후 push까지 수행:</p><pre><code class="language-text-x-trilium-auto">tjpub from-note dg4SLvEhyiJK --git --push
</code></pre><p>커밋 메시지 지정:</p><pre><code class="language-text-x-trilium-auto">tjpub from-note dg4SLvEhyiJK `
  --git `
  --push `
  --commit-message "Add post: Trilium metadata test"
</code></pre><p>커밋 메시지를 생략하면 자동 생성됩니다.</p><pre><code class="language-text-x-trilium-auto">Add post: 제목
Update post: 제목
</code></pre><p><code>--push</code>는 반드시 <code>--git</code>과 함께 사용해야 합니다.</p><h3><strong>Trilium noteId에서 발행</strong></h3><p>Trilium 노트에 메타데이터를 붙여두면 CLI에서 title/category/tag/slug를 매번 입력하지 않아도 됩니다.</p><pre><code class="language-text-x-trilium-auto">tjpub from-note dg4SLvEhyiJK --dry-run
</code></pre><p>발행:</p><pre><code class="language-text-x-trilium-auto">tjpub from-note dg4SLvEhyiJK --git --push
</code></pre><p>CLI 옵션이 Trilium 메타데이터보다 우선합니다.</p><pre><code class="language-text-x-trilium-auto">tjpub from-note dg4SLvEhyiJK `
  --slug custom-slug `
  --category Homelab `
  --tag proxmox `
  --dry-run
</code></pre><hr><h2><strong>7. Trilium 노트 작성 규칙</strong></h2><p>Trilium 노트에는 다음 라벨/속성을 붙이는 것을 권장합니다.</p><pre><code class="language-text-x-trilium-auto">#blog
#blog-ready
#slug=proxmox-install
#category=Homelab
#category=Proxmox
#tag=proxmox
#tag=pve
#tag=virtualization
#published=true
#pin=false
#mermaid=true
</code></pre><p>현재 지원되는 규칙:</p><figure class="table"><table><thead><tr><th style="border-bottom-style:solid;border-bottom-width:1px;border-color:rgba(255, 255, 255, 0.69);padding:5px 10px;">Trilium 메타데이터</th><th style="border-bottom-style:solid;border-bottom-width:1px;border-color:rgba(255, 255, 255, 0.69);padding:5px 10px;">Jekyll 반영</th></tr></thead><tbody><tr><td style="border-color:rgba(255, 255, 255, 0.18);padding:5px 10px;"><code>#slug=proxmox-install</code></td><td style="border-color:rgba(255, 255, 255, 0.18);padding:5px 10px;">파일명 slug</td></tr><tr><td style="border-color:rgba(255, 255, 255, 0.18);border-top-style:solid;border-top-width:1px;padding:5px 10px;"><code>#category=Homelab</code></td><td style="border-color:rgba(255, 255, 255, 0.18);border-top-style:solid;border-top-width:1px;padding:5px 10px;"><code>categories</code></td></tr><tr><td style="border-color:rgba(255, 255, 255, 0.18);border-top-style:solid;border-top-width:1px;padding:5px 10px;"><code>#tag=proxmox</code></td><td style="border-color:rgba(255, 255, 255, 0.18);border-top-style:solid;border-top-width:1px;padding:5px 10px;"><code>tags</code></td></tr><tr><td style="border-color:rgba(255, 255, 255, 0.18);border-top-style:solid;border-top-width:1px;padding:5px 10px;"><code>#published=false</code></td><td style="border-color:rgba(255, 255, 255, 0.18);border-top-style:solid;border-top-width:1px;padding:5px 10px;"><code>published: false</code></td></tr><tr><td style="border-color:rgba(255, 255, 255, 0.18);border-top-style:solid;border-top-width:1px;padding:5px 10px;"><code>#pin=true</code></td><td style="border-color:rgba(255, 255, 255, 0.18);border-top-style:solid;border-top-width:1px;padding:5px 10px;"><code>pin: true</code></td></tr><tr><td style="border-color:rgba(255, 255, 255, 0.18);border-top-style:solid;border-top-width:1px;padding:5px 10px;"><code>#mermaid=true</code></td><td style="border-color:rgba(255, 255, 255, 0.18);border-top-style:solid;border-top-width:1px;padding:5px 10px;"><code>mermaid: true</code></td></tr></tbody></table></figure><p>권장 본문 구조:</p><pre><code class="language-text-x-trilium-auto"># Proxmox VE 설치하기

본문 내용...

```mermaid
flowchart TD
    A[Trilium 초안] --&gt; B[tjpub]
    B --&gt; C[GitHub Pages]
```
</code></pre><p><code>--strip-first-h1</code>을 사용하면 첫 번째 H1 제목을 본문에서 제거할 수 있습니다.</p><pre><code class="language-text-x-trilium-auto">tjpub from-note dg4SLvEhyiJK --strip-first-h1
</code></pre><p>예정 기능:</p><ul><li><code>#blog-ready</code>가 없는 노트 발행 차단 옵션</li><li><code>#date=YYYY-MM-DD</code> 지원</li><li>Trilium 내부 note 링크를 블로그 URL로 변환</li></ul><hr><h2><strong>8. Jekyll/Chirpy 저장소 구조</strong></h2><p>대상 GitHub Pages 저장소는 다음 구조를 가정합니다.</p><pre><code class="language-text-x-trilium-auto">oats-lab.github.io/
├─ _config.yml
├─ _posts/
│  └─ 2026-06-15-proxmox-install.md
├─ assets/
│  └─ img/
│     └─ posts/
│        └─ proxmox-install/
│           ├─ image.png
│           └─ network.png
├─ Gemfile
└─ .github/
   └─ workflows/
</code></pre><p><code>tjpub</code>가 직접 수정하는 위치:</p><pre><code class="language-text-x-trilium-auto">_posts/
assets/img/posts/
</code></pre><p><code>config.yml</code>의 <code>jekyll.site_dir</code>는 실제 GitHub Pages 저장소 경로여야 합니다.</p><pre><code class="language-text-x-trilium-auto">jekyll:
  site_dir: "../oats-lab.github.io"
</code></pre><p><code>--git</code>을 사용할 경우 <code>site_dir</code>는 반드시 Git 저장소여야 합니다.</p><hr><h2><strong>9. 보안 주의사항</strong></h2><p>GitHub Pages 저장소는 Public일 수 있습니다. 공개 저장소에 민감 정보가 올라가면 즉시 노출됩니다.</p><p>절대 커밋하지 말아야 할 것:</p><ul><li>Trilium ETAPI 토큰</li><li>GitHub 토큰</li><li>비밀번호</li><li>private key</li><li><code>.env</code></li><li><code>config.yml</code></li><li>개인 메모 원본 전체 export</li><li>내부 IP, VPN 주소, 서버 접속 정보</li></ul><p><code>.gitignore</code>에 포함해야 할 항목:</p><pre><code class="language-text-x-trilium-auto">config.yml
.env
.env.*
*.key
*.pem
*token*
*secret*
</code></pre><p>발행 전 검증에서 다음 문자열은 경고 대상입니다.</p><pre><code class="language-text-x-trilium-auto">password
passwd
token
secret
api_key
private key
.env
</code></pre><p>경고도 실패로 처리하려면 <code>--strict</code>를 사용합니다.</p><pre><code class="language-text-x-trilium-auto">tjpub from-note dg4SLvEhyiJK --strict --dry-run
</code></pre><hr><h2><strong>10. 개발 로드맵</strong></h2><p>구현 완료:</p><ul><li>Python 패키지 구조</li><li>Typer 기반 CLI</li><li><code>from-file</code></li><li><code>from-note</code></li><li>이미지 처리</li><li>Trilium 첨부 이미지 다운로드</li><li>Front Matter 생성</li><li>발행 전 검증</li><li>Git commit/push 자동화</li><li>Trilium 메타데이터 파싱</li></ul><p>예정 기능:</p><ul><li>README 예제와 CLI 옵션 자동 검증</li><li>Trilium 내부 링크 변환</li><li><code>#blog-ready</code> 기반 발행 차단</li><li><code>#date=YYYY-MM-DD</code> 메타데이터</li><li>여러 노트 일괄 발행</li><li>GitHub Actions 배포 결과 확인</li><li>GUI 또는 Trilium Script Button 연동</li></ul><hr><h2><strong>11. 문제 해결</strong></h2><h3><code><strong>config.yml</strong></code><strong>을 읽지 못하는 경우</strong></h3><pre><code class="language-text-x-trilium-auto">tjpub check-config
</code></pre><p>다른 설정 파일:</p><pre><code class="language-text-x-trilium-auto">tjpub check-config --config config.example.yml
</code></pre><h3><strong>Trilium 토큰 환경 변수가 보이지 않는 경우</strong></h3><p>PowerShell:</p><pre><code class="language-text-x-trilium-auto">$env:TRILIUM_ETAPI_TOKEN
</code></pre><p>값이 비어 있으면:</p><pre><code class="language-text-x-trilium-auto">$env:TRILIUM_ETAPI_TOKEN="여기에_ETAPI_토큰"
</code></pre><h3><strong>ETAPI가 HTML을 반환하는 경우</strong></h3><p>다음 에러는 reverse proxy나 로그인 페이지가 ETAPI 요청을 가로챈 경우입니다.</p><pre><code class="language-text-x-trilium-auto">Trilium ETAPI returned HTML instead of JSON
</code></pre><p>해결:</p><ul><li><code>trilium.base_url</code> 또는 <code>trilium.api_base_url</code>을 ETAPI에 직접 접근 가능한 주소로 설정</li><li>reverse proxy에서 <code>/etapi/*</code> 요청이 Trilium으로 통과되도록 설정</li></ul><h3><strong>첨부 이미지가 GitHub Pages에서 깨지는 경우</strong></h3><p>본문에 이런 링크가 남아 있으면 배포 검증이 실패합니다.</p><pre><code class="language-text-x-trilium-auto">/api/attachments/...
</code></pre><p>최신 <code>tjpub from-note</code>는 이 경로를 ETAPI 첨부 다운로드로 처리합니다. 다시 생성하세요.</p><pre><code class="language-text-x-trilium-auto">tjpub from-note dg4SLvEhyiJK --overwrite --git --push
</code></pre><h3><strong>Git push 권한 오류</strong></h3><p>두 개 이상의 GitHub 계정을 사용한다면 SSH key를 계정별로 분리하는 것을 권장합니다.</p><p>예:</p><pre><code class="language-text-x-trilium-auto">Host github-oats-lab
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_oats_lab
</code></pre><p>블로그 저장소 remote:</p><pre><code class="language-text-x-trilium-auto">git remote set-url origin git@github-oats-lab:oats-lab/oats-lab.github.io.git
</code></pre><h3><code><strong>site_dir</strong></code><strong>가 Git 저장소가 아닌 경우</strong></h3><pre><code class="language-text-x-trilium-auto">Configured Jekyll site_dir is not a Git repository
</code></pre><p><code>config.yml</code>의 <code>jekyll.site_dir</code>가 실제 GitHub Pages 저장소 clone 경로인지 확인하세요.</p><hr><h2><strong>12. 라이선스</strong></h2><p>아직 라이선스 파일이 없습니다.</p><p>예정:</p><ul><li>개인용/공개용 범위를 정한 뒤 <code>LICENSE</code> 파일 추가</li><li>오픈소스로 공개할 경우 MIT License 검토</li></ul>