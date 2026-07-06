---
title: 图库
date: 2026-06-26 17:30:00
top_img: /img/homepage-bg.jpg
aside: false
comments: false
---

<style>
.gallery-categories {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.5rem;
  margin: 2rem 0;
}
.gallery-card {
  position: relative;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.gallery-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 24px rgba(0,0,0,0.15);
}
.gallery-card a {
  display: block;
  text-decoration: none;
}
.gallery-card img {
  width: 100%;
  height: 220px;
  object-fit: cover;
  display: block;
}
.gallery-card-title {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 1rem;
  background: linear-gradient(transparent, rgba(0,0,0,0.7));
  color: #fff;
  font-size: 1.5rem;
  font-weight: bold;
  text-align: center;
}
</style>

<div class="gallery-categories">
  <div class="gallery-card">
    <a href="/gallery/wallpaper/">
      <img src="/gallery/wallpaper/img/wallpaper-1.jpg" alt="壁纸">
      <div class="gallery-card-title">壁纸</div>
    </a>
  </div>
  <div class="gallery-card">
    <a href="/gallery/yihuan/">
      <img src="/gallery/yihuan/img/yihuan-1.jpg" alt="异环">
      <div class="gallery-card-title">异环</div>
    </a>
  </div>
</div>
