```html
<div class="container parent">
  <div class="middle child"></div>
  <div class="left child"></div>
  <div class="right child"></div>
</div>
```
```css
    * {
      margin: 0;
      left: 0;
      box-sizing: border-box;
    }

    .child {
      height: 500px;
    }

    .container {
      overflow: hidden;
    }

    .middle {
      float: left;
      width: 100%;
      padding-left: 200px;
      padding-right: 200px;
      background-color: brown;
    }

    .left {
      float: left;
      position: relative;
      width: 200px;
      margin-left: -100%;
      background-color: chocolate;
    }

    .right {
      float: left;
      position: relative;
      width: 200px;
      margin-left: -200px;
      background-color: darkcyan;
    }
```

use [live server](https://www.npmjs.com/package/live-server) to open [index.html](./index.html) on this folder.

### Key points
  - float盒子的左边缘会贴合容器左边缘或前一个float元素的右边缘, 一行内宽度不够会自动换行.
  - 负margin的应用, 本例中.left元素的负margin在不够自身宽度时仍然会处于第二行, 当等于大于自身宽度时, 会向上一行(即左margin决定了元素的起始摆放位置), `.left { margin-left: -100% }`时, .left元素与middle元素为一个起始位置.
  - 关于消除遮挡, 这里有两个做法, 第一个做法是给左盒子设置`{ position: relative; left: -200px; }`, 利用自身宽度挤开.middle, 因为默认的box-sizing是content-box; 第二个做法就是本例中的做法, 将盒子变成border-box, 再用内边距去挤开左右侧相同宽度即可.
