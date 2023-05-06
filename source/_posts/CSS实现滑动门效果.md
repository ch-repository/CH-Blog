---
title: CSS实现滑动门效果
date: 2023-05-06 16:09:21
tags: CSS
categories: 前端开发
comments: false
---

代码示例：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>滑动门效果</title>
    <style>
        html, body {
            margin: 0;
            padding: 0;
        }

        a {
            text-decoration: none;
            color: #000;
        }

        /* 滑动门 */
        .tab {
            position: relative;
            display: flex;
            justify-content: flex-start;
            align-items: center;
            width: 100vw;
            height: 60px;
            border-bottom: 2px solid #ccc;
            box-shadow: 0 2px 6px 0 rgba(9,23,29,0.21);
        }

        .tab-item {
            z-index: 2;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100%;
            width: 90px;
            padding: 0 10px;
        }

        /* 活跃样式：鼠标hover到指定item后，需要添加.active来切换活跃状态 */
        .active {
            color: #FFFFFF;
            transition: color .4s; /* 针对color来实现动画 */
        }

        /* 滑动门：鼠标hover到.active对应的元素后，滑动门也需要跟着切换位置 */
        .tab-active {
            z-index: 1;
            position: absolute;
            bottom: 0;
            width: 110px;
            height: 100%;
            display: inline-block;
            background: deepskyblue;
            transition: left .4s; /* 针对left变动来实现动画 */
        }
    </style>

</head>
<body>
    <!-- 
        滑动门实现思路：
        首先通过一个div包裹各个tab项，然后使用flex布局，做出横向的tab或者纵向的tab，
        最重要的是，我们利用tab-active来做滑动门的要素

        在tab-active上，我们使用了absolute+left进行定位，以区分不同选项的位置，并通过transition
        制造动画效果，让活跃态（背景色的切换），看起来像是划过去的
     -->

    <div class="tab">
        <a href="#" class="tab-item">Flex 布局</a>
        <a href="#" class="tab-item">下拉面板</a>
        <a href="#" class="tab-item">滑动门</a>

        <!-- 滑动门 -->
        <div class="tab-active"></div>
    </div>

    <script>
        window.onload = () => {
            /**
             * 移动切换状态
             * 1、获取所有元素
             * 2、移除上一个元素的active class
             * 3、添加当前hover元素的active class
             * 4、切换滑动门的left位置
            */

            // 上一个元素索引
            let activeTabIndex = 0;
            // 所有目录项
            const tabItems = document.querySelectorAll('.tab-item');
            // 滑动门
            const tabActive = document.querySelector('.tab-active');
            tabItems.forEach((item, index) => {
                item.onmouseover = e => {
                    tabItems[activeTabIndex].classList.remove('active');
                    e.target.classList.add('active');
                    // 切换活跃元素索引
                    activeTabIndex = index;
                    // 切换left已做滑动门
                    tabActive.style.left = `${index * 110}px`
                }
            })
        }
    </script>
</body>
</html>
```
