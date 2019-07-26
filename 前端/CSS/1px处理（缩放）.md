```css
.css-1px,
.css-1px-b,
.css-1px-l,
.css-1px-r,
.css-1px-t,
.css-1px-tb {
  position: relative;
}

.css-1px:before {
  content: " ";
  position: absolute;
  left: 0;
  top: 0;
  width: 200%;
  border: 1px solid #333;
  color: #c7c7c7;
  height: 200%;
  -webkit-transform-origin: left top;
  transform-origin: left top;
  -webkit-transform: scale(0.5);
  transform: scale(0.5);
}

.css-1px-t:before {
  top: 0;
  border-top: 1px solid #333;
  -webkit-transform-origin: 0 0;
  transform-origin: 0 0;
  -webkit-transform: scaleY(0.5);
  transform: scaleY(0.5);
}

.css-1px-b:before,
.css-1px-t:before {
  content: " ";
  position: absolute;
  left: 0;
  right: 0;
  height: 1px;
  color: #c7c7c7;
}

.css-1px-b:before {
  bottom: 0;
  border-bottom: 1px solid #333;
  -webkit-transform-origin: 0 100%;
  transform-origin: 0 100%;
  -webkit-transform: scaleY(0.5);
  transform: scaleY(0.5);
}

.css-1px-tb:before {
  content: " ";
  position: absolute;
  left: 0;
  top: 0;
  width: 200%;
  border-top: 1px solid #333;
  border-bottom: 1px solid #333;
  color: #c7c7c7;
  height: 200%;
  -webkit-transform-origin: left top;
  transform-origin: left top;
  -webkit-transform: scale(0.5);
  transform: scale(0.5);
}
.css-1px-l:before {
  left: 0;
  border-left: 1px solid #333;
  -webkit-transform-origin: 0 0;
  transform-origin: 0 0;
  -webkit-transform: scaleX(0.5);
  transform: scaleX(0.5);
}

.css-1px-l:before,
.css-1px-r:before {
  content: " ";
  position: absolute;
  top: 0;
  width: 1px;
  bottom: 0;
  color: #c7c7c7;
}

.css-1px-r:before {
  right: 0;
  border-right: 1px solid #333;
  -webkit-transform-origin: 100% 0;
  transform-origin: 100% 0;
  -webkit-transform: scaleX(0.5);
  transform: scaleX(0.5);
}
```