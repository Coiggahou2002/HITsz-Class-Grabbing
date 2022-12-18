# HITsz-Class-Grabbing
HITsz 计科限选课抢课脚本，By [Coiggahou](https://github.com/Coiggahou2002)

## 使用

1. 在下方代码中**修改目标课程名**，**填写选课开放起始时间**
2. 提前进入选课系统 - 学生选课 - 【限选】标签页
3. 按 `F12` (部分笔记本是 `Fn` + `F12`) 打开浏览器开发者工具，点击 `Console` 标签页
4. 将脚本粘贴到控制台，按回车执行，然后把浏览器放在一旁，不要关闭
5. 在选课开放之后，可以自行查看是否成功选上

> 注：该脚本主要是为了避免抢课高峰期人工刷新页面的操作迟钝的问题，笔者水平有限，逻辑粗糙，不对选课成功性作保证，需要者可自行取用or修改

```javascript
const config = {
  // 想抢的课名
  coursesToEnroll: ["自然语言处理", "人工智能", "服务计算"],

  // 抢课开始时间
  // 以 2022-12-16 13:00:00 为例
  year: 2022,
  month: 12,
  day: 16,
  hour: 13,
  minute: 0,
  second: 0,
};

// 以下不改动
function grab() {
  try {
    Array.from(document.querySelectorAll(".ivu-table-body tr"))
      .filter((row) => {
        const courseNameCell = row.cells[1];
        const tag = courseNameCell.querySelector("div p a");
        const courseName = tag.innerText;
        return config.coursesToEnroll.includes(courseName);
      })
      .forEach((row) => {
        const enrollBtn = row.cells[11].querySelectorAll("div button")[1];
        enrollBtn.removeAttribute("disabled");
        enrollBtn.click();
      });
  } catch (error) {
    console.error("Sorry, 抢课出现错误", error);
  }
}

const timestamp = new Date(
  year,
  month - 1,
  day,
  hour,
  minute,
  second
).getTime();

// 每 30ms 检查一次是否到点，到点就开抢
const timer = setInterval(() => {
  const now = new Date().getTime();
  if (now >= timestamp) {
    grab();
    clearInterval(timestamp);
  }
}, 30);
```
