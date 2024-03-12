# CSS

由于组件化的流行，写 CSS 的机会变少，再加上 CSS 本身属性记忆性的，工具性质的知识，因此整体来说考察的难度不会很高。

## 隐藏元素的方式有哪些？各自有什么优缺点？

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>隐藏元素</title>
    <style>
      .container {
        width: fit-content;
        border: 1px solid black;
        margin: 10px 0;
        display: flex;
        flex-direction: column;

        button {
          width: 200px;
          height: 60px;
        }
      }

      .display {
        display: none;
      }

      .visibility {
        visibility: hidden;
      }

      .opacity {
        opacity: 0;
      }

      .position {
        position: fixed;
        left: -100px;
        top: -100px;
      }
    </style>
  </head>
  <body>
    <button class="display">display: none</button>
    <div class="container">
      visibility: hidden
      <button class="visibility" onclick="alert('visibility: hidden')"></button>
    </div>
    <div class="container">
      opacity: 0
      <button class="opacity" onclick="alert('opacity: 0')"></button>
    </div>
    <button class="position">position</button>
  </body>
</html>
```

|                    | 占据空间 | 性能影响 | 可交互 | 支持过渡 |
| ------------------ | -------- | -------- | ------ | -------- |
| display: none      | no       | 回流重绘 | no     | no       |
| visibility: hidden | yes      | 重绘     | no     | no       |
| opacity: 0         | yes      | 重绘     | yes    | yes      |
| position           | no       | 回流重绘 | no     | no       |
