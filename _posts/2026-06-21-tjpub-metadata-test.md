---
title: "TJPUB - Trilium metadata test"
date: 2026-06-21 00:00:00 +0900
categories: [Python]
tags: [python, trilium, githubpages]
mermaid: true
---

<p><code>from-note</code>가 이제 Trilium 노트의 속성/태그 메타데이터를 읽어서 slug/category/tag와 일부 Front Matter 옵션을 자동으로 채웁니다.</p><p><strong>Trilium 노트에 붙일 메타데이터</strong></p><pre><code class="language-text-x-trilium-auto">#blog
#blog-ready
#slug=proxmox-install
#category=Homelab
#category=Proxmox
#tag=proxmox
#tag=pve
#tag=virtualization
#published=true
#pin=false
#mermaid=true</code></pre><p>지원되는 값:</p><ul><li><code>#slug=...</code> → 파일명 slug</li><li><code>#category=...</code> → <code>categories</code></li><li><code>#tag=...</code> → <code>tags</code></li><li><code>#published=false</code> → front matter에 <code>published: false</code></li><li><code>#pin=true</code> → front matter에 <code>pin: true</code></li><li><code>#mermaid=true</code> → front matter에 <code>mermaid: true</code></li><li><code>#blog</code>, <code>#blog-ready</code> → 현재는 파싱만 하고, 이후 필터링/발행 조건에 활용 가능</li></ul><p><strong>CLI 옵션과 메타데이터 우선순위</strong><br>CLI 옵션이 항상 Trilium 메타데이터보다 우선합니다.</p><p>예:</p><pre><code class="language-text-x-trilium-auto">tjpub from-note note123 --slug cli-slug --category CLI --tag cli</code></pre><p>노트에 <code>#slug=metadata-slug</code>가 있어도 결과는 <code>cli-slug</code>가 됩니다.</p><p><strong>사용 예시</strong><br>메타데이터를 노트에 잘 붙여둔 경우:</p><pre><code class="language-text-x-trilium-auto">tjpub from-note "note_id_here" --dry-run</code></pre><p>일부만 CLI로 덮어쓰기:</p><pre><code class="language-text-x-trilium-auto">tjpub from-note "note_id_here" `
  --slug custom-slug `
  --category Homelab `
  --dry-run</code></pre><p>Git commit까지:</p><pre><code class="language-text-x-trilium-auto">tjpub from-note "note_id_here" --git</code></pre><p>&nbsp;</p><p>mermaid 테스트:</p><pre spellcheck="false"><code class="language-mermaid">flowchart TB
A --&gt; B
B --&gt; C</code></pre><p>&nbsp;</p><p>이미지 테스트</p><figure class="image image_resized" style="width:50%;"><img style="aspect-ratio:1254/1254;" src="/assets/img/posts/tjpub-metadata-test/profile.png" width="1254" height="1254" alt=""></figure>