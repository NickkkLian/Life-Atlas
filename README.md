# Life Atlas · 旅居与吃喝记录

世界地图上按**省/州**点亮你去过 / 旅居 / 长居的地区，记录每座城市的多维评分、攻略、必去景点，以及餐厅、Bar（含 cocktail）、其他店铺，并提供餐厅 / Bar 排行榜。

代码公开托管，**个人数据在你的私有 repo 的 `life.json`**，通过本机输入的 GitHub fine-grained token 读写。

## 仓库结构

```
公开 repo（开 GitHub Pages）
├── index.html        ← 应用
└── regions.geojson   ← 省/州 + 国家边界（用于按地区点亮地图）

私有 repo（数据）
└── life.json
```
> `index.html` 和 `regions.geojson` 必须放在同一目录（应用会同源加载 regions.geojson）。

## 部署

1. 把 `index.html` 和 `regions.geojson` 放进**公开 repo**，Settings → Pages 开启。
2. 把 `life.json` 放进**私有 repo**（或留空，应用首次保存自动创建）。
3. 建 **Fine-grained token**：对该私有 repo 授予 **Contents: Read and write**。
4. 打开 Pages 网址 → ⚙ 填 owner / 私有仓库名 / 路径(`life.json`) / token → 连接并加载。

## 用法

- **地图按地区点亮**：中国城市点亮所在**省份**；其他国家点亮**整个国家**。同一地区有多城时按**最高级别**取色（有长居→长居色）。颜色：长居（品红）/ 旅居（青）/ 去过（橙）。
- **城市单点**：放大到一定级别后，地图上出现脉冲发光的城市单点（可点击进入），缩小时只看地区色块。
- **添加城市**：左侧输入城市名，从下拉点一下即可，自动定位并点亮所在地区。
- **我的城市**：可折叠（☰ 或 ‹）；按 长居 → 旅居 → 去过 分组，同组按拼音排序。
- **城市详情**：点城市进入**全屏**页。顶部切换状态；评分维度（4–6，⚙ 可改）配雷达图；攻略/必去/评价/备注。
- **店铺**：一个添加入口（选 餐厅 / Bar / 其他类型）。餐厅、其他为**表格**；Bar 仿 Excel——每家 bar 一块，下面列出 cocktail。每条都有类别下拉，可随时改归类（分错了一键纠正）。
- **Cocktail**：点 chip 循环 默认 → 喜欢(青) → 特别喜欢(品红)；菜品标 **菜**。
- **排行榜**：全屏三列 餐厅 / Bar / 连锁，可切 总榜 / 分地区（连锁与地区无关，恒显全部）。未评分的店排在每列最底部并标「待评分」。
- **连锁**：不作为城市，只在排行榜的「连锁」列出现（数据存 life.json 顶层 `chains`）。
- **保存**：按钮或 `Ctrl/Cmd+S` 写回 `life.json`。

## 数据格式（life.json）

```jsonc
{
  "version": 1,
  "config": {
    "dimensions": ["美食","气候环境","性价比","交通便利","安全感","综合宜居"],
    "otherTypes": [{ "label":"咖啡/茶","emoji":"☕" }]
  },
  "cities": [{
    "id":"", "name":"", "country":"", "lat":0, "lng":0,
    "status":"visited | sojourned | resided",
    "scores":{}, "attractions":"", "guide":"", "review":"", "notes":"",
    "restaurants":[{ "id":"","name":"","rating":8,"note":"" }],
    "bars":[{ "id":"","name":"","rating":9,"note":"",
      "cocktails":[{ "id":"","name":"","level":"tried|liked|loved","food":true }] }],
    "others":[{ "id":"","name":"","type":"密室逃脱","rating":8,"note":"" }]
  }]
}
```
