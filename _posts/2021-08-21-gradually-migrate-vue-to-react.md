---
layout: post
title: Gradually Migrate from Vue to React
image: assets/images/posts/gradually-migrate-vue-to-react.jpg
author: pnnutkung
categories: [programming]
tags: [vue, react, javascript]
---

หลังจากบริษัทรวมทีม dev เข้าด้วยกัน ผมได้มีโอกาสเข้ามาช่วยทำ Frontend ที่เป็น Vue แต่ว่าในทีม Frontend มีคนที่เขียนเป็นจำนวนน้อยเพราะเราใช้ React และเรามี library หลายอย่างที่ใช้กันภายในโดย support React เป็นหลัก เราเลยตัดสินใจว่าจะ migrate project Vue เป็น React จะช่วยให้ maintain code ได้ง่ายขึ้น

## Vuera

ผมลองหา library ที่จะช่วยให้ Vue กับ React ใช้งานด้วยกันได้ ก็ได้มาเจออันนึงที่ชื่อว่า [vuera](https://github.com/akxcv/vuera) ค่อนข้างน่าสนใจ แต่เขาไม่ได้พัฒนาต่อมานานแล้ว เลยตัดสินใจไม่ใช้ดีกว่า

## Back to basic

สุดท้ายเลยถาม[มิตรสหายท่านหนึ่ง](https://github.com/ReiiYuki)และได้คำตอบว่าเราใช้ท่านี้กันดีกว่า  
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

```jsx
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

สิ่งที่เราจะทำ

1. สร้าง Vue component ส่วน template ให้สร้าง element ที่มี ref ชื่อตามที่เราต้องการ
2. สร้าง project ใหม่ที่เป็น React component library แยกออกมาจาก project หลักที่เป็น Vue  
   ใช้ ReactDOM render component ลงไปใน target ที่ pass เข้ามาทาง parameter
3. import component React เข้ามาใช้ใน Vue script โดยต้องทำใน lifecycle mounted

code ส่วนที่เป็น Vue component สร้าง `<div ref="app"></div>` ตรง `app` จะใส่เป็นชื่ออะไรก็ได้ที่เราต้องการ  
ส่วน script ให้ import React component  
ตรง parameter ใส่ target เป็น ref ของ element ที่เราจะให้ render React ลงไป

```vue
<template>
  <div ref="app"></div>
</template>

<script>
import HelloWorld from "@component/react"; // ตรงนี้ import library React ที่เราสร้างขึ้นมาเอง

export default {
  mounted() {
    HelloWorld({ target: this.$ref.app }); // ถ้าอยาก pass ค่าอะไรไปใช้ใน React ก็สามารถเพิ่มลงไปได้
  },
};
</script>
...
```

ฝั่งที่เป็น React ให้ทำเป็น export function

```jsx
import { render } from "react-dom";
import React from "react";

const Component = () => <div>Hello World</div>;

export default ({ target }) => render(<Component />, target);
```

## Summary

วิธีนี้ช่วยให้เราค่อย ๆ เปลี่ยน code ส่วนที่เป็น Vue เป็น React ได้ โดยไม่ต้องเปลี่ยนทีเดียวทั้งหมด  
user ที่ใช้งานอาจจะไม่สังเกตเห็นความเปลี่ยนแปลง เรื่อง performance ตอนนี้ที่ลองใช้มายังไม่ได้รู้สึกว่าแย่หรือช้า
