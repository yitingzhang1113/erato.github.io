---
layout: page
title: About
permalink: /about/
---

<style>
  .about-container {
    max-width: 720px;
    margin: 50px auto;
    padding: 35px 30px;
    background: linear-gradient(135deg, #a8dadc 0%, #f1faee 100%);
    border-radius: 25px;
    box-shadow: 0 10px 40px rgba(72, 201, 176, 0.25);
    font-family: "Comic Sans MS", "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
    color: #264653;
    line-height: 1.75;
    position: relative;
    overflow: hidden;
  }
  .about-container::before {
    content: "✨";
    font-size: 3rem;
    position: absolute;
    top: 15px;
    right: 20px;
    animation: sparkle 2.5s infinite alternate ease-in-out;
  }
  @keyframes sparkle {
    0% { opacity: 0.3; transform: rotate(0deg) scale(1); }
    100% { opacity: 1; transform: rotate(15deg) scale(1.2); }
  }
  .about-container h2 {
    color: #1d3557;
    font-weight: 900;
    text-align: center;
    margin-bottom: 25px;
    letter-spacing: 1.2px;
    text-shadow: 1px 1px 2px #a8dadc;
  }
  .about-container p {
    font-size: 1.15rem;
    margin-bottom: 20px;
    text-indent: 2em;
  }
  .about-container strong {
    color: #2a9d8f;
  }
  .about-container em {
    color: #e76f51;
    font-style: normal;
  }
  .about-container a {
    color: #e63946;
    font-weight: 700;
    text-decoration: none;
    border-bottom: 2px dashed #e63946;
    transition: all 0.3s ease;
  }
  .about-container a:hover {
    color: #f1faee;
    background-color: #e63946;
    border-bottom: 2px solid #f1faee;
    padding-bottom: 2px;
    border-radius: 4px;
    text-decoration: none;
  }
  .about-footer {
    margin-top: 45px;
    font-size: 0.95rem;
    color: #718096;
    text-align: center;
    font-style: italic;
  }
</style>

<div class="about-container">
  <h2>关于我 💫</h2>

  <p>你好呀！我是 <strong>Erato</strong>，一名活力满满的 <em>Georgia Tech</em> 计算机科学硕士（MSCS）学生，爱探索科技世界的奇妙奥秘。</p>

  <p>我现在在学习数据结构前端开发，还有时下最火的检索增强生成模型（RAG）。喜欢用项目驱动学习，不断提升技能，也乐于和大家分享我的成长与收获。</p>

  <p>欢迎来到我的
    <a href="https://github.com/yourusername" target="_blank" rel="noopener noreferrer">GitHub</a>，
    或者我的
    <a href="https://yourusername.github.io" target="_blank" rel="noopener noreferrer">个人博客</a>，
    一起玩转代码与创意，探索无穷可能！</p>

  <div class="about-footer">
    <p>这里的内容均为原创，版权属于我本人，未经允许请勿转载。如需授权，欢迎随时联系我～</p>
  </div>
</div>
