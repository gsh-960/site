---
hide:
  - navigation # 显示右
  - toc #显示左
  - footer
  - feedback
comments: false
---




<center><font class="custom-font ml3">海天生存指北</font></center>
<style>
    .custom-font {
    font-size: 31px; /* 默认字体大小为8px */
    color: #757575;
}
@media (max-width: 768px) { /* 假设768px及以下为移动端 */
    .custom-font {
        font-size: 25px; /* 移动端字体大小为6px */
    }
}
</style>

<div class="grid cards" markdown>

-   :material-notebook-edit-outline:{ .lg .middle } __导航栏__

    ---
    ![image](https://pic3.zhimg.com/80/v2-0786a6086793ccca444226e9ab3561ec_1440w.webp){ class="responsive-image" align=right width="230" height="300" style="border-radius: 25px;" }
    

    - [x] 𝕙𝕒𝕧𝕖 𝕒 𝕘𝕠𝕠𝕕 𝕥𝕚𝕞𝕖 !
    === "Mac/PC端"
        请在上方标签选择分类/左侧目录选择文章
    === "移动端"
        请点击左上角图标选择分类和文章

</div>
<style>
    @media only screen and (max-width: 768px) {
        .responsive-image {
            display: none;
        }
    }
</style>

>不同于市面上过时的MkDocs教程，本站提供了最详细最便捷最前沿的MkDocs中文文字/视频教程，与[官方发布](https://squidfunk.github.io/mkdocs-material/changelog/)的教程版本同步。包含了MkDocs的安装、配置、主题美化、插件使用等内容。无论你是初学者还是有经验的用户，都能在这里找到你需要的帮助。我们还提供了示例和实用的技巧，帮助你更好地使用MkDocs。𝓳𝓾𝓼𝓽 𝓮𝓷𝓳𝓸𝔂 𝓲𝓽～

***  

<!-- <strong>推荐文章:material-book:</strong>

  - [利用Mkdocs部署静态网页至GitHub pages](blog/Mkdocs/mkdocs1.md)
  - [Mkdocs部署配置说明(mkdocs.yml)](blog/Mkdocs/mkdocs2.md)
  - [如何给MKdocs添加友链](blog/websitebeauty/linktech.md)
  - [网站添加Mkdocs博客](blog/Mkdocs/mkdocsblog.md)
  - [Blogger](blog/index.md) -->



<div class="grid cards" markdown>

-   :simple-materialformkdocs:{ .lg .middle } __必看__

    ---

    - [像老乡鸡一样做饭](https://cooklikehoc.soilzhu.su/){target=“_blank”}(外链)



-   :simple-aboutdotme:{ .lg .middle } __关于__

    ---

</div>



[^Knowing-that-loving-you-has-no-ending]:太阳总是能温暖向日葵  
[^see-how-much-I-love-you]:All-problems-in-computer-science-can-be-solved-by-another-level-of-indirection


<style>
body {
  position: relative; /* 确保 body 元素的 position 属性为非静态值 */
}

body::before {
  --size: 35px; /* 调整网格单元大小 */
  --line: color-mix(in hsl, canvasText, transparent 80%); /* 调整线条透明度 */
  content: '';
  height: 100vh;
  width: 100%;
  position: absolute; /* 修改为 absolute 以使其随页面滚动 */
  background: linear-gradient(
        90deg,
        var(--line) 1px,
        transparent 1px var(--size)
      )
      50% 50% / var(--size) var(--size),
    linear-gradient(var(--line) 1px, transparent 1px var(--size)) 50% 50% /
      var(--size) var(--size);
    mask: linear-gradient(-20deg, transparent 50%, white);
  top: 0;
  transform-style: flat;
  pointer-events: none;
  z-index: -1;
}
  

@media (max-width: 768px) {
  body::before {
    display: none; /* 在手机端隐藏网格效果 */
  }
}
</style>
<head> 
  <!-- Umami Analytics -->
  <script defer src="https://cloud.umami.is/script.js" data-website-id="061b4dea-9b7b-4ffa-9071-74cde70f3dfb"></script>
</head>