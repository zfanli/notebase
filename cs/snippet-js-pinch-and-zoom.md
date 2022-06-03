---
type: Snippet
tags: JavaScript QoK-D
---

# Snippet: Pinch and Zoom

双指缩放算法

重点：

- [[javascript-Math.hypot|Math.hypot()]] 计算两点距离
- [[javascript-Math.atan2|Math.atan2()]] 计算两点弧度

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Gesture</title>
    <style>
      * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
      }
      html {
        touch-action: none;
      }
      body {
        min-height: 100vh;
        min-width: 100vw;
      }
      #tar {
        background-color: red;
        min-width: 5px;
        min-height: 5px;
        position: fixed;
        top: 50%;
        left: 50%;
        transform-origin: 0 0;
      }
    </style>
  </head>
  <body>
    <h1>Gesture</h1>
    <div>touches count: <span id="count">0</span></div>
    <div>rawDis: <span id="rawDis">0</span></div>
    <div>newDis: <span id="newDis">0</span></div>
    <div>ratio: <span id="ratio">0</span></div>
    <div>rad: <span id="rad">0</span></div>
    <div>top: <span id="top">0</span></div>
    <div>left: <span id="left">0</span></div>
    <div id="tar"></div>

    <script src="https://libs.cdnjs.net/lodash.js/4.17.10/lodash.js"></script>
    <script>
      const tar = document.getElementById('tar')
      const countEl = document.getElementById('count')
      const rawDisEl = document.getElementById('rawDis')
      const newDisEl = document.getElementById('newDis')
      const ratioEl = document.getElementById('ratio')
      const radEl = document.getElementById('rad')
      const topEl = document.getElementById('top')
      const leftEl = document.getElementById('left')
      let count = 0

      const updateCount = _.debounce((n) => (countEl.innerHTML = n), 25, {
        maxWait: 25,
      })

      const rad = (p1, p2) => Math.atan2(p2.y - p1.y, p2.x - p1.x)
      const dis = (p1, p2) => Math.hypot(p2.y - p1.y, p2.x - p1.x)

      let rawDis = null

      document.body.addEventListener('touchstart', (e) => {
        const touches = e.touches
        if (touches.length > 1) {
          rawDis = dis(
            { x: touches[0].clientX, y: touches[0].clientY },
            { x: touches[1].clientX, y: touches[1].clientY }
          )
          rawDisEl.innerHTML = rawDis
        }
      })

      document.body.addEventListener('touchmove', (e) => {
        const touches = e.touches
        updateCount(++count)
        if (touches.length > 1) {
          const newDis = dis(
            { x: touches[0].clientX, y: touches[0].clientY },
            { x: touches[1].clientX, y: touches[1].clientY }
          )
          const curRad = rad(
            { x: touches[0].clientX, y: touches[0].clientY },
            { x: touches[1].clientX, y: touches[1].clientY }
          )

          // const startP = touches[0].clientX >= touches[1].clientX ? 1 : 0
          const startP = 0

          newDisEl.innerHTML = newDis
          ratioEl.innerHTML = newDis / rawDis
          radEl.innerHTML = curRad
          topEl.innerHTML = touches[startP].pageY + 'px'
          leftEl.innerHTML = touches[startP].pageX + 'px'

          tar.style.width = newDis + 'px'
          tar.style.left = touches[startP].pageX + 'px'
          tar.style.top = touches[startP].pageY + 'px'
          tar.style.transform = `rotate(${curRad}rad)`
        }
      })
    </script>
  </body>
</html>
```
