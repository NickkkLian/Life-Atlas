# Life Atlas · 旅居与吃喝记录

一个单文件网页应用：世界地图上点亮你去过 / 旅居 / 长居的城市，记录每座城市的多维评分、攻略、必去景点、评价，以及餐厅、Bar 和喝过的 cocktail。

代码公开托管，**个人数据存放在你的私有 repo 的 `life.json`**，通过你本机输入的 GitHub fine-grained token 读写，token 只存在浏览器 localStorage，不会进入任何代码。

## 仓库结构

```
公开 repo（开 GitHub Pages）
└── index.html        ← 整个应用，单文件

私有 repo（存数据，可与你其他数据库共存）
└── life.json         ← 本应用的数据文件
```

## 部署（约 3 分钟）

1. 把 `index.html` 放进**公开 repo**，在 repo 的 Settings → Pages 选 `main` 分支根目录，开启 Pages。等几十秒拿到网址。
2. 把 `life.json`（可用本仓库附带的示例，或留空让应用首次保存时自动创建）放进你的**私有 repo**。
3. 创建一个 **Fine-grained personal access token**：
   - https://github.com/settings/personal-access-tokens/new
   - **Resource owner** 选你的账号；**Repository access** 选 *Only select repositories* → 勾选那个私有 repo。
   - **Permissions → Repository permissions → Contents** 设为 **Read and write**。
   - 生成后复制 `github_pat_...`（只显示一次）。
4. 打开你的 Pages 网址，点右上角 **⚙**，填入 owner、私有仓库名、文件路径（`life.json`）、token，点「连接并加载」。

## 用法

- **添加城市**：在左侧搜索框输入城市名（联想来自 OpenStreetMap）选中即可定位添加；或直接点击地图任意位置打点。
- **三种状态**：去过（金）/ 旅居 >1 月（青）/ 长居（珊瑚红）。点城市详情顶部切换；右上图例可按状态过滤地图与列表。
- **评分**：每城 4–6 个维度（可在 ⚙ 里改名增删，4–6 个之间），滑杆调分，右侧雷达图实时更新。
- **攻略 / 必去 / 评价 / 备注**：四个文本框。
- **餐厅 & Bar**：两个独立列表，各自单一 0–10 评分 + 一句话备注。
- **其他店铺**：合并成一个列表（桌游 / 密室逃脱 / 剧本杀 / 足浴按摩 / 水疗温泉 / KTV / 咖啡馆…），每条带一个类型下拉标签；类型清单可在 ⚙ 里增删改 emoji 与名称。
- **Cocktail**：在 Bar 里输入喝过的酒名；点标签循环 `默认 → 喜欢（青色高亮）→ 特别喜欢（珊瑚红高亮 ★）`；标签上的 ✕ 删除。
- **保存**：点「保存到 GitHub」或按 `Ctrl/Cmd + S`，写回 `life.json`。离开页面前若有未保存改动会提示。

## 数据格式（life.json）

```jsonc
{
  "version": 1,
  "config": {
    "dimensions": ["美食", "气候环境", "性价比", "交通便利", "安全感", "综合宜居"],
    "otherTypes": [ { "label": "桌游", "emoji": "🎲" }, { "label": "密室逃脱", "emoji": "🔓" } ]
  },
  "cities": [
    {
      "id": "...", "name": "东京", "country": "日本",
      "lat": 35.68, "lng": 139.65,
      "status": "visited | sojourned | resided",
      "scores": { "美食": 9.5, "...": 0 },
      "attractions": "", "guide": "", "review": "", "notes": "",
      "restaurants": [ { "id": "", "name": "", "rating": 8, "note": "" } ],
      "bars": [ {
        "id": "", "name": "", "rating": 9, "note": "",
        "cocktails": [ { "id": "", "name": "Negroni", "level": "tried | liked | loved" } ]
      } ],
      "others": [ { "id": "", "name": "", "type": "密室逃脱", "rating": 8, "note": "" } ]
    }
  ]
}
```

私有 repo 里其他数据库的文件互不影响——本应用只读写它认到的这一个 `life.json`。

## 说明

- 数据是私有的：没有 token 的访客打开页面只看到空地图与连接提示，看不到你的数据。
- token 只存浏览器本地；换设备需重新填一次。
- 地图瓦片用 CARTO 暗色底图，地名联想用 OpenStreetMap Nominatim，均无需 key。
