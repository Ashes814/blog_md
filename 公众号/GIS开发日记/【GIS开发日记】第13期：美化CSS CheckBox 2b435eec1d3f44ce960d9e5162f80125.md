# 【GIS开发日记】第13期：美化CSS CheckBox

> *分享GIS开发面经八股，GIS基础（地图投影，空间分析，矢量栅格，地图算法），前端（HTML，CSS，JS/TS，React），开源地图库（Leaflet，Openlayers，Cesium），服务端（JAVA SE，GeoServer），数据库（MySQL，PostgreSQL，PostGIS，MongoDB）等知识技巧*
> 

# 如何美化CheckBox

- `label` 标签中的`for`属性指向`value1`，是第一个`input`的`id`，表明这个标签是标注`value1`的，点击这个`label`时，`checked`的状态也会改变

```html
<input type="checkbox" name="timeType" value="1" id="value1" checked="checked"/>
<label for="value1"></label>
```

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC13%E6%9C%9F%EF%BC%9A%E7%BE%8E%E5%8C%96CSS%20CheckBox%202b435eec1d3f44ce960d9e5162f80125/Untitled.png)

- 设计`label`的样式

```css
/* 当checkbox被选中，改变相邻标签的颜色和背景 */
#value1:checked+label{
    color:blue;
    background: #4cda60;
}

#value1+label{
          cursor: pointer;
          color:red;
          display: block;
          width:60px;
          height: 30px;
          background: #fafbfa;

          /* 设计椭圆形toggle */
          border-radius: 15px;
          position: relative;
          box-shadow:inset 0 0 0 0 #eee,0 0 1px rgba(0,0,0,0.4);
         
          /* 设置动画变换 */
          transition: background 0.1s;

          /* 兼容性 */
          -webkit-transition: background 0.1s;
          -moz-transition: background 0.1s;
          -o-transition: background 0.1s;
      }
```

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC13%E6%9C%9F%EF%BC%9A%E7%BE%8E%E5%8C%96CSS%20CheckBox%202b435eec1d3f44ce960d9e5162f80125/Untitled%201.png)

- 添加`before`伪元素，制作一个白色的拖拽按钮

```css
#value1+label:before{
          content:'';
          position: absolute;
          background: #fff;
          top:1px;
          left:1px;
          width: 28px;
          height: 28px;
          border-radius: 50%;
          box-shadow:0 3px 1px rgba(0,0,0,0.05), 0 0 1px rgba(0,0,0,0.3);
          transition: left 0.1s;
          -webkit-transition: left 0.1s;
          -moz-transition: left 0.1s;
          -o-transition: left 0.1s;
      }
```

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC13%E6%9C%9F%EF%BC%9A%E7%BE%8E%E5%8C%96CSS%20CheckBox%202b435eec1d3f44ce960d9e5162f80125/Untitled%202.png)

- 调整位置并隐藏原来的`input`即可

```css
/* 隐藏input */
      #value1{
              display: none;
      }

/* 移动 */
#value1:checked+label:before{
    left:31px;
}
```

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC13%E6%9C%9F%EF%BC%9A%E7%BE%8E%E5%8C%96CSS%20CheckBox%202b435eec1d3f44ce960d9e5162f80125/Untitled%203.png)