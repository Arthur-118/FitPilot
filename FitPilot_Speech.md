# FitPilot — PPT 配套演讲稿

> **用途**：outline 配套 PPT（17 页）的讲解稿。英文正文按页给出，直接照着念即可；**中文批注**（> 开头）说明本页要点、应强调的词、衔接过渡，以及"老师可能追问"的答题提示。
> **总时长参考**：约 8–10 分钟（每页 20–40 秒）；若时间紧，批注标了「可压缩」的页可一带而过。
> 演示时记得翻到对应页再讲，动作之间空 1–2 秒，别赶。

***

## 开场白（翻到 P1 封面前）

```
Good morning / afternoon, everyone. Today I will present our Android app project, FitPilot —
an AI fitness coach. I will walk you through the idea, what similar apps are missing,
our new features, the UI design, and finally our work plan.
```

> 📌 **中文**：开场三句话讲清"我是谁、讲什么、顺序是什么"。

***

## P1 · 封面

```
FitPilot — your AI fitness coach that sees, thinks, and tracks.
```

> 📌 **中文**：只念这一句，然后停 1 秒，让标题/口号先被看到。这句话是全片的中心论点，后面所有内容都是它的展开。

***

## P2 · FitPilot Overview（Idea at a Glance）

```
So what is FitPilot? Put simply, it is a personal coach in your pocket. You do not pick a
workout course yourself — FitPilot decides what you should train today, watches you while
you do it, and remembers your progress for tomorrow. Three abilities drive this:
it thinks — it combines weather, your recovery state and your preferences to build today's plan;
it sees — the phone camera, with on-device AI, counts your reps and checks your form;
and it tracks — all running and strength data is saved in one local database.
```

> 📌 **中文**：核心是"三会"——会想(think)、会看(see)、会记(track)。讲到三个 it 开头的动作时，手指一下屏幕上对应的三个色块。

***

## P3 · The Gap We Address（痛点→解法）

```
Here is the problem we want to solve. First, many people do not know what to train today —
too many choices, no guidance. Second, when you train alone, nobody watches your form, and
that can lead to injury. Third, your data is scattered — running and strength live in
separate apps. FitPilot answers all three: a daily recommendation that is weather- and
recovery-aware; on-device pose AI that checks your form; and one unified log that makes
tomorrow's plan smarter.
```

> 📌 **中文**：三对"痛点 → 解法"一一对应屏幕上的三行对照卡。
> 💬 **可能的追问**："那和其他健身 App 相比，你们到底哪里不同？"→ 直接答：大多数 App 要你自己选课且不同运动数据割裂，我们是"替你决定 + 全程纠正 + 数据闭环"三合一，而且全部本地可解释。

***

## P4 · Related Apps & Their Limitations（同类 App 对比）

```
Let me show you what already exists. Fitbod generates daily plans from muscle recovery,
but it ignores the weather and gives you no live form feedback. Keep is a big Chinese
platform, but running and strength are two separate systems, and its AI scoring runs
on the server. Good Habits — which is our chosen open-source base — counts reps on device
very well, but it does no recommendation at all. And HeartRunner handles running well,
but only running.
```

> 📌 **中文**：表上 4 个 App 各给一句"它强在哪 + 缺什么"。最后落到一句总结："each covers one corner — none of them combines all three."（每个都只做到了一部分）。
> 💬 **可能的追问**："你们和 Keep 的 AI 计分有何区别？"→ Keep 的 AI 在服务端、要联网；我们的姿势识别在手机上本地运行，离线免费，且每一步都能解释为什么这么判。
> 💬 **可能的追问**："为什么不直接改 Good Habits？"→ 我们就是基于它扩展的——但它的天花板是"只计数"，我们要加上天气推荐、恢复评分、语音反馈和跑步闭环，这就构成了差异化。

***

## P5 · Open Source We Build Upon（开源基座）

```
To be efficient, we reuse open source instead of starting from zero. Good Habits is our
primary base — we take its pose-estimation pipeline, its Room data layer, and its
Compose UI patterns. HeartRunner shows us how to connect a BLE heart-rate strap via GATT.
For wger, we only reference its data structure and copy no code, because it is AGPL
licensed. The exercise dataset itself is bundled locally, so the app works fully offline.
```

> 📌 **中文**：强调两层信息——① 我们不是从零写，这是"高效开发者"的正确做法（评分项 Open Source 15%）；② 我们清楚许可证边界（AGPL 只参考不复制），显专业。
> 💬 **可能的追问**："你们到底复用了多少代码、自己写了多少？"→ 答：基线结构复用，但新增的推荐引擎、恢复评分、语音反馈、跑步闭环都是我们自己的代码，Git 提交记录可查。详见文档。

***

## P6 · New Features at a Glance（功能总览）

```
So what do we actually add on top of the base? Eight features in total: an equipment-aware
daily recommendation engine; a recovery score; real-time pose form checking; weather and
air-quality awareness; voice coaching with HIIT and cool-down; a running session with GPS
and heart rate; one unified closed loop across all scenarios; and jump-rope auto counting.
```

> 📌 **中文**：快速带过 8 个功能卡，不用逐条展开——目的是给听众"地图"，后面 7-10 页会挑重点详讲。念到屏幕左上角编号顺序，手在屏幕上从上往下扫即可。【可压缩：若时间紧，一句带过】

***

## P7 · Daily Recommendation Engine（推荐引擎 · Processing 得分主力）

```
Let me explain the most important feature: the daily recommendation engine. When you
first open the app, a short quiz builds your profile — your training scene, the equipment
you own, your goal, frequency, and experience level. Meanwhile, every exercise in the
library carries tags: its type, equipment, muscle group, and difficulty. The engine works
in two stages. First, a hard filter: if the equipment does not match your inventory, the
exercise is removed. Then, a weighted score: weather, recovery, your preference, and
muscle balance each contribute, and the top three exercises become today's plan.
```

> 📌 **中文**：**重点页，讲慢**。突出两个词——"hard filter（硬过滤）"和"weighted score（加权评分）"。强调我们用的是**可解释的规则**，不是黑盒 AI：每个推荐都能说出"为什么"。这是应对老师"勿提交无法解释的 AI 代码"警告的关键。
> 💬 **可能的追问**："为什么不直接用机器学习做推荐？"→ ① 没有足够训练数据（单个用户几十条记录）；② 每一条推荐都要能解释（可解释性）；③ 规则引擎第一天就能用，冷启动友好；而"AI"我们用在了它真正擅长的地方——视觉识别（第 9 页）。

***

## P8 · Recovery Score（恢复评分）

```
The second feature is a recovery score from 0 to 100. Four inputs feed into it: sleep hours,
your resting heart rate, a fatigue check-in, and your recent training load. Each input is
mapped by simple rules into a sub-score, then weighted — sleep forty percent, heart rate
twenty-five, fatigue twenty, and training load fifteen. Above eighty, we train hard; below
forty for three days, the app suggests a rest day.
```

> 📌 **中文**：讲清公式逻辑即可（0.40+0.25+0.20+0.15=1）。强调"数据来源分层"：睡眠/疲劳是打卡表单不需要硬件，心率可以用 BLE 带也可以手动输入——**没有心率带也能用**。
> 💬 **可能的追问**："心率不准确怎么办？"→ 我们比对的是用户自己的基线（相对值），不是绝对标准；没设备就走手动/打卡，功能完整可用。

***

## P9 · On-Device Pose Coach（姿态教练 · AI 得分点子）

```
Here is where we actually use AI. The model is TensorFlow Lite with MoveNet, and it runs
entirely on the phone — offline, free, and explainable. It counts every rep, and by
computing joint angles at the shoulder, elbow, and knee, it judges whether a rep is done
correctly. If not, the app speaks: "keep your elbows tucked." It can also extend to
squats, planks, and jump-rope counting.
```

> 📌 **中文**：**重点页**。为什么本地 AI 有三重价值：①离线免费②无隐私顾虑③可解释。💬 **可能的追问**："移动端能跑得动吗？"→ MoveNet 是 Google 为移动设备优化的轻量模型，Good Habits 已证明在普通安卓上实时可用，我们复用其管线。

***

## P10 · Running & The Closed Loop（跑步 + 闭环）

```
The fourth feature is running, and more importantly — the closed loop. During a run, GPS
records distance and pace, and a BLE heart-rate strap feeds a live curve. And here is the
loop: today's workout goes into the database, the recovery score updates, and tomorrow's
recommended intensity adjusts accordingly. Even the air quality matters — if the AQI is
polluted, an outdoor run automatically becomes an indoor plan.
```

> 📌 **中文**：强调"闭环"——这是把各功能串成整体、体现 Data Integration(10%) 的关键。讲"closed loop"时手指沿屏幕上的回环图标转一圈。
> 💡 **加分句**（可自由加）："So the app actually gets smarter the more you use it."

***

## P11 · UI — Onboarding（入门问卷）

```
Now let me walk you through the UI. On first launch, a question wizard builds your
profile — one step per screen, so it is quick and easy. These answers power the
recommendation filter we discussed earlier, from day one.
```

> 📌 **中文**：本页及后三页是 **UI 评分 20%** 的主力。用一句"these answers power the recommendation filter from day one"把 UI 和核心功能挂钩，避免"只是好看"。

***

## P12 · UI — Today's Plan Home（首页今日计划）

```
The home screen answers one question: what should I do today? You see the recovery score
as a ring, a weather and air-quality banner that explains why a session is indoors, and
three recommended cards with sets and reps. Everything a user needs is one tap away.
```

> 📌 **中文**：突出两处设计巧思——①"恢复评分圆环"一眼可知今天状态；②"天气/AQI 横幅带原因文案"（不是只告诉你改室内，还告诉你为什么），这叫 transparency（透明度），也呼应推荐的可解释性。

***

## P13 · UI — Strength Session（力量训练会话）

```
During a strength session, the camera shows the pose skeleton overlaid on your body. The
rep counter is big, easy to read mid-workout, and each rep shows its form status. On top
of the visuals, the voice channel announces corrections, so you can keep your eyes on the
exercise.
```

> 📌 **中文**：**重点 UI 页**。三要点：骨架叠加、大字号计数器、语音通道（eyes stay on the exercise）。这里就是第 9 页姿态教练的"交互呈现"。
> 💬 **可能的追问**："摄像头隐私怎么办？"→ 全本地处理，不上传任何画面；处理完即弃，符合隐私优先原则。

***

## P14 · UI — Running Page & Unified Stats（跑步 + 统计）

```
The running screen shows a live heart-rate curve, distance, pace, and the map track.
And on the stats page, everything comes together: training volume, distance, recovery
trend, and your streak — all in one place, filterable by scenario.
```

> 📌 **中文**：用"everything comes together"承接第 10 页的闭环——统计页是闭环的"外显"。强调图表用图表不用裸表格（呼应评分 Data Output 25%）。

***

## P15 · Tech Stack（技术栈）

```
A quick look at the stack. Every choice has a reason. QWeather for weather and air quality —
free and accessible in mainland China. TensorFlow Lite and MoveNet for pose AI — on-device
and offline. Room for the database, Jetpack Compose for the UI, Android BLE for heart rate,
and built-in TextToSpeech for voice — no network needed. The whole thing is free, local,
and demo-safe.
```

> 📌 **中文**：**逐行念太快会像报菜名**——建议重点讲 3 个卖点即可：①QWeather 国内免费免翻墙 ②全链路离线 ③TTS 免费内置。其余扫读。
> 💬 **可能的追问**："为什么用 Room 不用 Firebase？"→ 我们的数据是个人训练记录，本地就够且隐私更好；不依赖网络也保证演示稳定。

***

## P16 · Work Plan（工作计划）

```
Finally, our work plan. Week six, we scaffold the project on Good
Habits. Alpha lands at the end of week seven with the recommendation engine and basic
logging. Weeks eight to ten build the pose AI and running, so Beta — with full form feedback
and the running loop — lands at the end of week ten. Weeks eleven to thirteen polish:
recovery score, AQI, unified charts, and edge cases. Final submission is week fourteen.
```

> 📌 **中文**：节奏是"Alpha=第7周末、Beta=第10周末、Final=第14周"。**强调三个里程碑**比念每周任务更重要——手依次点三处里程碑标记。
> 💬 **可能的追问**："时间这么紧，万一做不完怎么办？"→ 课程允许按实际工作量评分；我们已经规划了可裁剪的加分项（跳绳/HIIT/拉伸放第 13 周），核心闭环六周内先保证。

***

## P17 · Thank You（结束）

```
That is our project — FitPilot. It decides what you train, watches your form, and makes
tomorrow smarter. Thank you — and I am happy to take questions.
```

> 📌 **中文**：收尾三句复述"会想/会看/会记"，与开场呼应。
> 💬 **Q\&A 兜底**：如果被问住，不要编造——"That's a good question. I'll need to confirm the detail, but the general approach is..." 诚实比硬答更有分。

***

