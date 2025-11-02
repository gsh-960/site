---
hide:
  - navigation # 显示右
  - toc #显示左
  - footer
  - feedback
comments: false
---




<center><font class="custom-font ml3">牛马生活实录</font></center>
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

>𝓳𝓾𝓼𝓽 𝓮𝓷𝓳𝓸𝔂 𝓲𝓽～

***

<div class="grid cards" markdown>

-   :simple-materialformkdocs:{ .lg .middle } __必看__

    ---

    - [像老乡鸡一样做饭](https://cooklikehoc.soilzhu.su/){target=“_blank”}(外链)

-   :simple-aboutdotme:{ .lg .middle } __关于__

    ---
    -   <div id="restaurant-picker" style="text-align: center; padding: 10px;"><button onclick="pickRestaurant()" style="background-color: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer; font-weight: bold;">今天吃什么</button><div id="result" style="margin-top: 15px; font-size: 18px; min-height: 50px;"></div></div>
</div>

<script>    
const restaurants = [
        "食堂",
        "穷鬼小炒",
        "烧鸭",
        "汤粉",
        "重庆小面",
        "木桶饭"
    ];

    function pickRestaurant() {
        if (restaurants.length === 0) {
            document.getElementById("result").innerHTML = "餐厅列表为空!";
            return;
        }

        // 随机选择一家餐厅
        const chosen = restaurants[Math.floor(Math.random() * restaurants.length)];

        // 生成推荐指数 (0-100的随机数)
        const recommendationIndex = Math.floor(Math.random() * 101);

        // 显示结果
        document.getElementById("result").innerHTML = `今天就吃 <strong>${chosen}</strong> 吧！<br>推荐指数: ${recommendationIndex}%`;
    }
</script>
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