---
layout: post
title: Gradually Migrate from Vue to React
image: assets/images/posts/gradually-migrate-vue-to-react.jpg
---

หลังจากบริษัทรวมทีม dev เข้าด้วยกัน ผมได้มีโอกาสได้เข้ามาช่วยทำ Frontend ที่เป็น VueJS แต่ว่าในทีม Frontend ก็มีคนที่เขียนเป็นจำนวนน้อย เพราะทีม Frontend เราใช้ React และเรามี library หลายอย่างที่ใช้กันภายในโดย support React เป็นหลัก เราเลยตัดสินใจว่าจะ migrate project VueJS เป็น React จะช่วยให้ maintain ง่ายขึ้น

## Vuera

ผมลองหา libray ที่จะช่วยให้ Vue กับ React ใช้งานด้วยกันได้ ก็ได้มาเจอ lib นึงที่ชื่อว่า [vuera](https://github.com/akxcv/vuera) ค่อนข้างน่าสนใจ แต่เขาไม่ได้พัฒนาต่อมานานแล้วก็เลยตัดสินใจไม่ใช้ดีกว่า

## Back to basic

สุดท้ายก็เลยถาามมิตรสหายท่านนึงและได้คำตอบว่าเราใช้ท่านี้กันดีกว่า เรียกว่าเป็น basic ของการเขียน React เลยก็ว่าได้

อย่างที่เรารู้กัน React จะ render component ใส่เข้าไปใน DOM ที่เรากำหนดไว้

**ตัวอย่าง** ถ้าเราใช้ Create React App สร้าง project ขึ้นมาใหม่ ใน folder `public/index.html` code หน้าตาจะประมาณข้างล่างนี้

```html
<html>
  ...
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
  </body>
</html>
```

สังเกตว่าจะมี `<div id="root"></div>` เอาไว้ให้ `ReactDOM` render component ใส่ลงไป  
ดู code ใน `src/index.js` ประกอบ

```javascript
...
ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
...
```

จาก code จะเห็นได้ว่าเราจะ render component `<App />` ใส่ลงไปใน element id root  
[อ่าน document เกี่ยวกับ ReactDOM.render ได้ที่นี่](https://reactjs.org/docs/react-dom.html#render)

เราสามารถใช้ประโยชน์จาก function นี้ทำให้ React ไปใช้งานบน Vue ได้

## Vue with React

สิ่งที่เราจะทำ ให้ Vue render element ที่มี ref และให้ ReactDOM render component ลงไป

code ส่วนที่เป็น Vue component สร้าง `<div ref="app"></div>` ตรง app จะใส่เป็นอะไรก็ได้ที่เราต้องการ

```vuejs
<template>
  <div ref="app">
  </div>
</template>

<script>
import HelloWorld from "@component/react";

export default {
  mounted() {
    HelloWorld({ target: this.$ref.app });
  }
}
</script>
...
```

ฝั่งที่เป็น React ให้ทำเป็น export function

```jsx
const Component = <div>Hello World</div>;
export default ({ target }) => render(<Component />, target);
```
