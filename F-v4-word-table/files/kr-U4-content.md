# 冒险 v3 U4｜两区内容表与数据草案

> 配套交付：`KR-U4-冒险数据-2026-10-20.json`按区顺序存放同形区对象数组；如接线器要求与U1一样的单对象根，使用本目录`分区JSON/`中的对应文件。数组仅是多区打包容器，区对象不新增字段。

2026-10-20；按已定稿 U1 样板与上传素材编制，供逐区内容验收。

**分成两区：A1「运动搭档」覆盖 L6 全部 9 个必会词；A2「文具集合」覆盖 L7 全部 8 个必会词。** 共 17 词，不重不漏。两课的场景分别围绕 playground 与 classroom，本来就有独立课本羁绊；分区可让孩子先掌握运动中的护后排、相邻加速，再在文具中看见护邻居和相邻回血。两区均不依赖另一课必会词充当对手搭档，避免为凑阵容提前引入未学词。没有把同课拆成零散小关。

每区六类基础场景＋一个头目，另有完整替换的单招／单棋子变体。每单元两条头目规则分别落在两区：A1 真实 `swim` 开场护盾；A2 沿 U1 的开场技能冷却归零路径。规则各用一次，不叠成一个头目。

配套数据为同形区对象 `分区JSON/KR-U4-A1-冒险数据-2026-10-20.json`、`分区JSON/KR-U4-A2-冒险数据-2026-10-20.json`；单元汇总文件按区顺序收纳这些对象。新增草案，不导入，不修改 `cards/bonds/skills/context-sentences`。本稿未读到运行中的游戏源码，也没有实战平衡结论。

## 1. 本次读到的附件与草案适配边界

依据：上传的《1-冒险核心v3.md》《2-样板-第一单元内容表.md》《3-样板-第一单元数据.json》《4-素材-总表.md》《4-素材-kr-U4.md》。机械参数、课号和必会身份按 U4 素材；结构、字段和接线合同沿 U1。U1 文档里记载的仓库阅读和实测安排属于原样板，不作为本稿已执行的事实。

| 样板入口 | 本稿对应字段 | 接入时必须做到 |
|---|---|---|
| 单一区词表 | `unit=4`、`area=1/2`、`lessons=[6]/[7]`、`requiredWords` | 每区分母取本区 9／8 词；两区并集 17，不把绿叶计入 |
| `SCENES` 与帮助文字 | 空 0 项＋1–7 项；每枚 `[word,r,c]`，`SCENE_HELP`同索引 | 基础与完整变体二选一；同一族动作不叠加 |
| 配方和前置 | `sceneRules`、`sceneVariants`、`requiresObserved` | 按实际走过且真实展示的战斗筛选；未选支线和地图预览无效 |
| 起步与合法供应 | `starter`、`leafAccess` | 起步只送使用权与首手，不认证学会；白名单绿叶不出任何题 |
| 敌方效果牌 | `enemyActions` | `swim`真实贴敌方 climb，沿原机制开场护盾 15；不改成一枚站着挨打的棋子 |
| 头目 | `boss` | A1 `ruleType=enemy-card`，不改开场技能冷却；A2 `ruleType=environment`，双方技能冷却 0；各只施行一次 |
| 句子 | `questionPolicy` | 附件无完整句库，`approvedContextRefs=[]`；接入时才按同一 word 键、`approved=true`和原 needs 核验已有条目 |

`schemaVersion=1`仍表示草案版本，不能当作程序已支持声明。保留 U1 区对象的全部顶层键和嵌套字段；A1 非环境头目的 `skillOpeningCooldownMs=null`表示不施加该环境改动。没有增加技能核、群体护盾或新数值字段。`freeUnit=false`只沿用 U4 现有单元权限，不新增付费救援；进入可用单元后的救援始终免费。

## 2. 17 个必会词：身份、出现方式与已有句子

### 2.1 A1／L6「运动搭档」：9 词

| 词 | 课／现有 kind | 对手如何让孩子看见 | 主要救援场景 | 套样板条款 |
|---|---|---|---|---|
| catch | L6 object | 给唯一相邻队友 kick 护盾 15；扩到多人时仍选相邻当前 HP 最低者 | A1-S4 | U1 样板§3、§4、§6：真实相邻、牌题同词 |
| climb | L6 object | 前排保护同列后一格 kick；斜后方不成立 | A1-S1／S6／S7 | 同上 |
| dance | L6 object | 在 kick 正后方，加速这一位正交邻居 | A1-S5 | 同上 |
| fly | L6 object | 在已知 climb 保护下，按原 `enemy-back-lowest / damage / 16`选目标 | A1-S6 | 同上；不把目标类型扩写成保证斩杀 |
| jump | L6 object | 与 playground 正交相邻，显示已有课本羁绊；原一片伤害照常 | A1-S2／S7 | 同上；语块及出处见§3 |
| kick | L6 object | 作已知前排输出；原 `enemy-front / damage / 16`，技能间隔 3000ms | A1-S1／S4／S5／S7 | 同上 |
| ski | L6 object | 只替换 kick，climb 护后排已见；原前排伤害 7、技能间隔 2500ms | A1-S1-ski | 同上；技能间隔不冒充普攻节奏 |
| slide | L6 object | 与 playground 相邻，引用已有羁绊；原后排选择保留 | A1-S3 | 同上；语块及出处见§3 |
| swim | L6 effect | 敌方给自己的 climb 真贴牌，开场护盾 15；实际生效后弹本牌图 | A1-S1-swim／S7 | U1 样板§4：真动作、真来源；不占棋位 |

### 2.2 A2／L7「文具集合」：8 词

| 词 | 课／现有 kind | 对手如何让孩子看见 | 主要救援场景 | 套样板条款 |
|---|---|---|---|---|
| crayon | L7 object | notebook 的相邻回血已见，只新增对敌前排 `slowNext` | A2-S5-crayon | U1 样板§3、§4、§6：真实配方、牌题同词 |
| eraser | L7 object | notebook 照常给相邻队友回血，只新增敌前排 `daze` | A2-S5 | 同上；不宣称控制＋输出已证协同 |
| glue stick | L7 object | 在同一已知回血搭档旁，只新增 `enemy-neighbours / slowNext` | A2-S5-glue-stick | 同上；不是全体减速 |
| notebook | L7 object | 给相邻当前 HP 最低的队友回血 12；基础配方中只有一个合法邻居 | A2-S2／S5／S6及变体 | 同上；真回血后才算展示 |
| pencil | L7 object | 被笔袋护住，自己按原后排最低血目标选择投掷 | A2-S1／S2／S7 | 同上 |
| pencil case | L7 object | 先单教相邻护盾，再教相邻 classroom 的原课本羁绊 | A2-S1／S3／S7 | 同上；先护邻居后地点羁绊 |
| ruler | L7 object | 已知 notebook 护理下打敌前排，原伤害 12、技能间隔 5000ms | A2-S6 | 同上；不编尺子更远或更强 |
| scissors | L7 object | 与 classroom 正交相邻，显示原课本羁绊；原重击 24照常 | A2-S4／S7 | 同上；语块及出处见§3 |

17 是可救、可供应和可继续学习的完整覆盖量，不是必须一趟答 17 题，也不是答对一次就学会。每个词的题仍由现有学习引擎按这个词的真实阶段选择。

**已有句子边界：** 本稿不写新英文句子，不复制未提供句库中的句子，也不把未核验条目标为已批准。配套 JSON 的 `approvedContextRefs`为空；接入方只能对本场选中的同一必会词，从 `kr/context-sentences.json`选 `approved=true` 且原 needs 满足的条目，否则保留既有看图／听音／拼写入口。以下四段仅引用素材中的原羁绊语块，不扩写成句子题。

## 3. 绿叶撑场景：每区一张显式白名单

| 区／绿叶 | 课本／游戏归属 | 为什么搭在一起更强（机械依据） | 只引用已有语块及出处 | 在本区怎样给孩子用 |
|---|---|---|---|---|
| A1／playground | U4 L6，非必会 place | jump与它正交相邻触发现有 bond；不另外推算伤害倍率 | `jump on the playground`；U4素材“课本羁绊及借词”，原 `bonds.json jump/playground`；p73（蹦床插图） | 开区给免题场景使用权；救回 jump 后下场保证 jump，并给 playground 合法选择 |
| A1／playground | 同上；仍为同一张白名单牌 | slide与它正交相邻触发另一条原 bond | `slide on the playground`；原 `bonds.json slide/playground`；p22、p73、p99 插图 | 救回 slide 后同样提供搭档选择；只在已走路径见过前置后使用此配方 |
| A2／classroom | U4 L7，非必会 place | pencil case邻接 classroom触发原 bond；笔袋本身的邻接护盾已先教 | `pencil case in the classroom`；原 `bonds.json pencil case/classroom`；p86–87（做笔袋手工页） | 开区给免题场景使用权；救回 pencil case 后保留本词供应，并给 classroom 选择 |
| A2／classroom | 同上；仍为同一张白名单牌 | scissors邻接 classroom触发原 bond | `scissors in the classroom`；原 `bonds.json scissors/classroom`；p87 插图 | 救回 scissors 后下场给同样配对机会 |

以上出处是附件记录的原出处，本稿没有重新访问教材页或替素材增加句子批准状态。`bondRefs`保留样板的 `{object,place}`两字段，页码和引用集中列在这张表。

**A1 白名单仅 `playground`；A2 白名单仅 `classroom`。** `juice`、`pool`虽同为 U4 绿叶，但本稿不借；更不为启用 `orange/juice`或 `ball/pool`把 U3 必会 orange、U1 必会 ball 当绿叶。也不把 A1 的 swim、dance、climb 当 A2 绿叶。玩家以前真实学会的词按 v3 原规则可用、会休息，不因本稿变成新词教学对象。

绿叶是实际在场单位，原技能正常运行；买、出、合成、领奖与羁绊入口均不考绿叶，不换考旁边另一个词，不计本区分母、不写学会、不获学习赐福。免题场景权限不等于解锁其他单元的学习内容。绿叶选择不挤掉新救必会词的下场保底。

## 4. 每区六类小场景＋墨水怪：一次只教一个新东西

坐标沿 U1：`r=0`前排 F、`r=1`后排 B，`c=0–4`对应 1–5 列。上下左右相邻，斜角无效。tuple 顺序即 uid 顺序，效果小图不占棋位；绿叶占棋位。普通均为 2 枚，精英 2 枚，头目 4 枚。

### 4.1 A1 六类基础配方与头目

| 场景／类型 | 最终敌阵 | 唯一新东西 | 必须在已走路径中见过 | 实际展示与救援候选 |
|---|---|---|---|---|
| **A1-S1 攀爬搭档护后排**／普通 | climb F3、kick B3（2枚） | `climb-rear` | 无；本区先从此开始 | climb只给同列后一格kick护盾18；不护斜后方。kick照常打敌前排。 可救：climb／kick |
| **A1-S2 操场跳一跳**／普通 | jump F3、playground B3（2枚） | `jump-playground` | A1-S1 | 只新增jump邻playground的既有羁绊；来源p73。场所原攻击保留，羁绊效果沿原数据，不编倍率。 可救：jump |
| **A1-S3 操场滑一滑**／普通 | slide F3、playground B3（2枚） | `slide-playground` | A1-S2 | 只新增slide邻playground的既有羁绊；来源p22、p73、p99。slide按enemy-back-lowest选目标，不宣称控制配合或必定命中某人。 可救：slide |
| **A1-S4 接住身边伙伴**／普通 | catch B3、kick F3（2枚） | `catch-adjacent-guard` | A1-S1 | catch给相邻当前HP最低的队友护盾15；这份两枚配方唯一相邻队友为kick。不是全队最低，也不加自己。 可救：catch／kick |
| **A1-S5 跟着节拍出手**／普通 | dance B3、kick F3（2枚） | `dance-adjacent-haste` | A1-S1 | dance只加速上下左右邻居kick，斜角无效；haste的持续和倍率沿引擎，不从amount=0推断。 可救：dance／kick |
| **A1-S6 飞过前排找目标**／精英 | climb F3、fly B3（2枚） | `fly-back-target` | A1-S1 | 已知climb继续护正后排fly；只新增fly原enemy-back-lowest、damage16。没有合法后排时按现行引擎处理，未真实展示后排命中不记新概念。 可救：climb／fly |
| **A1-S7 淘气墨水怪：运动搭档集合**／头目 | climb F1、kick B1、jump F4、playground B4（4枚） | 零新机制；拼已见组合与已见 swim | A1-S1、A1-S2、A1-S1-swim | 复习climb护kick与jump邻playground；swim只给climb开场护盾15。两组放第1与第4列，互不相邻；不加全体护盾或额外先手。 可救：climb／kick／jump／swim |

### 4.2 A1 单招／单棋子变体：替换整份配方

| 变体 | 敌阵／唯一新增动作 | 已见前置 | 本场可救词 |
|---|---|---|---|
| A1-S1-swim | climb F3、kick B3；只新增swim贴climb，按原效果开场shield15；climb护kick是已见组合。只在实际护盾应用时弹swim卡图。 | A1-S1 | climb／kick／swim |
| A1-S1-ski | climb F3、ski B3；climb同列后排护盾是复习；只新增ski原技能enemy-front、damage7、interval2500ms，不将技能间隔冒充普攻速度。 | A1-S1 | climb／ski |

### 4.3 A2 六类基础配方与头目

| 场景／类型 | 最终敌阵 | 唯一新东西 | 必须在已走路径中见过 | 实际展示与救援候选 |
|---|---|---|---|---|
| **A2-S1 笔袋护着铅笔**／普通 | pencil case F3、pencil B3（2枚） | `pencil-case-guard` | 无；本区先从此开始 | pencil case给上下左右邻居pencil护盾10；铅笔依原enemy-back-lowest选择目标，不需要额外控制连招。 可救：pencil case／pencil |
| **A2-S2 翻页照顾身边伙伴**／普通 | notebook B3、pencil F3（2枚） | `notebook-adjacent-heal` | A2-S1 | notebook给相邻当前HP最低队友回血12；只有pencil一个相邻队友。目标满血不算有效回血展示，不能以闪字代替真实血量恢复。 可救：notebook／pencil |
| **A2-S3 笔袋回到教室**／普通 | pencil case F3、classroom B3（2枚） | `pencil-case-classroom` | A2-S1 | 已见笔袋邻接护盾照常运转；只新增pencil case邻classroom的原羁绊，p86–87做笔袋手工页。 可救：pencil case |
| **A2-S4 教室里的小剪刀**／普通 | scissors F3、classroom B3（2枚） | `scissors-classroom` | A2-S1 | 只新增scissors邻classroom原羁绊，p87插图；scissors原重击和classroom原攻击保留，不另造剪贴增伤。 可救：scissors |
| **A2-S5 橡皮擦让前排愣住**／普通 | eraser F3、notebook B3（2枚） | `eraser-front-daze` | A2-S2 | notebook继续按相邻最低当前HP给eraser回血；只新增eraser原enemy-front daze。没有证据证明必定形成打断或斩杀链，不作此承诺。 可救：eraser／notebook |
| **A2-S6 尺子守在前面**／精英 | ruler F3、notebook B3（2枚） | `ruler-front-skill` | A2-S2 | notebook相邻回血是复习；只新增ruler原enemy-front damage12、技能间隔5000ms。它与classroom同机制，不另编更长射程。 可救：ruler／notebook |
| **A2-S7 淘气墨水怪：手工课集合**／头目 | pencil case F1、pencil B1、scissors F4、classroom B4（4枚） | `classroom-bell` | A2-S1、A2-S4 | 复用笔袋护铅笔、剪刀邻教室两组；唯一新规则为双方现存棋子开场技能冷却0，普攻不变。两组第1与第4列，笔袋不邻教室，不额外触发第三羁绊。 可救：pencil case／pencil／scissors |

### 4.4 A2 单招／单棋子变体：替换整份配方

| 变体 | 敌阵／唯一新增动作 | 已见前置 | 本场可救词 |
|---|---|---|---|
| A2-S5-crayon | crayon F3、notebook B3；notebook相邻回血是复习；只新增crayon原enemy-front slowNext。没有说明持续时长，按现有引擎，不添加减速倍率。 | A2-S2 | crayon／notebook |
| A2-S5-glue-stick | glue stick F3、notebook B3；notebook相邻回血是复习；只新增glue stick原enemy-neighbours slowNext，目标范围按现有引擎，不变成全体。 | A2-S2 | glue stick／notebook |

A2 没有本课效果牌，不从其他单元搬一张未学会效果牌来凑“单招”。两种变体各替换一枚新棋子，复用已见 notebook 邻接回血；这是样板中的“单辅助替换”同一做法。`enemyActions=[]`，不会假弹一张效果牌。

### 4.5 帮助关系、单项教学与排序合同

- **真实帮助都有直接依据。** climb→kick/fly/ski 依据 `ally-behind / guard /18`，同列后一格；catch→kick 依据 `ally-lowest / guard /15`，须是相邻最低当前 HP；dance→kick 依据 `ally-neighbours / haste`，正交相邻；pencil case→pencil 依据 `ally-neighbours / guard /10`；notebook→pencil/eraser/crayon/glue stick/ruler 依据 `ally-lowest / heal /12`，相邻最低当前 HP。上述配对均列在 U4 素材“实际可成立的帮助配对”中。
- **最低血不写成永远指定谁。** 两枚配方给辅助留下唯一合法邻居，所以对象清楚；若以后扩到多人，必须重新遵循相邻集合、实时 HP 和原部署顺序破同分。notebook 的目标满血时不假装回血演出；只有真实恢复后才记作已展示，不能从头顶图标直接写前置。技能间隔、持续与目标失效回退沿引擎，不从素材缺项里猜。
- **控制牌只讲原招。** eraser 是敌前排 daze，crayon 是敌前排 slowNext，glue stick是 enemy-neighbours slowNext；有 notebook 回血支持，但不把“控制＋输出”说成素材已经证明的收益或同目标连招。fly、slide、pencil 的后排选择按原 target，玩家无合法后排目标时的回退不在附件中，不能把尚未真实发生的后排命中登记为已展示。
- **先教，再添一项，最后拼旧组合。** A1 先 S1；swim 变体须已见 climb 护后排，头目还须已见 jump/playground 和实际 swim 开场护盾。A2 先 S1；笔袋地点羁绊须先见普通邻接护盾，控制变体须先见 notebook 的真实相邻回血。前置不满足先排那个简单配方，不因层号到了而强开头目。
- **原技能全部保留。** 减少首次教学混乱靠配方和前置，不把已有技能静音。A2-S3 有已见笔袋护盾＋唯一新增笔袋/教室羁绊；A2头目把笔袋/铅笔放 F1/B1、剪刀/教室放 F4/B4，笔袋到教室曼哈顿距离 4，不会偷偷再触发笔袋/教室第三条羁绊。
- **一场只选一份。** `sceneVariants`不是补丁叠加；S1-swim 和 S1-ski不能合并，S5-eraser/crayon/glue stick也只能选一份。普通首教最多一个新增组合／招，间隔复习和 A1头目复用可以零新增。
- **组合体验账本与学习账本分开。** 真实完成战斗且看见效果才记 observed；答错救援也可能已经看见该组合，但不会因此获使用权或认证学会。未选路线、预览、被动看牌、敌方施法不替玩家答题。保存同区续救状态，读档不重复施法。
- **错后隔两战再遇。** 第 b 战答错，b+1、b+2两场完整战斗不把该词作救援焦点，第 b+3战回同一配方、同词、零新招。补给和地图点击不算战斗。继续错就按新的 b再排；区末用续段承接，不因头目结束永久丢失目标。所有焦点均从本次真实配方 `rescueCandidates`取。

### 效果动作和 44px 弹图的落点

A1 的唯一效果牌是 swim：先形成最终敌方配方并完成本场原强度缩放，再沿原 deploy/开场初始化将护盾 15落实到 climb uid1；真实生效后在 climb 头顶弹一次 swim 本牌 44px 图。基础 climb 护身后与自身 swim开场盾是两条独立真实来源，不把 15改成群体效果、回血、免伤次数或永久血上限。不能在 JSON手写额外 shield再执行 deploy造成双加；读档缓存本次执行状态。

敌方拥有私有使用权／牌与动作状态，不消耗玩家的牌、能量或学习记录。A1头目已预先演示过 swim，因此是在组合复用中再次使用；A2环境规则只显示皇冠／铃声小标，不冒充 swim或 hello卡的成功事件。保留 v3 原战斗速度，不因弹图开启慢镜。

## 5. 头目规则选择：本单元恰好两条，分区各一条

| 规则／所在区 | 本场真实执行 | 开启前必须实际见过 | 必须排除的伪效果 |
|---|---|---|---|
| **规则1「游泳前先护好」／A1** | 敌方将原 swim贴在 climb上，开场护盾15。四枚队伍为 climb F1、kick B1、jump F4、playground B4；保护与地点组合复用 | A1-S1 climb护后排、A1-S2 jump/playground、A1-S1-swim真实开场盾 | 不给全队15盾，不加护盾倍率，不按技能冷却0开场，不对玩家偷施法；boss元信息本身不能再执行一次swim |
| **规则2「手工课铃响啦」／A2** | 双方最终在场单位（含绿叶）技能冷却设0；普攻冷却不变。四枚队伍为 pencil case F1、pencil B1、scissors F4、classroom B4 | A2-S1笔袋护邻居、A2-S4剪刀/classroom。铃声本身是本场唯一新增环境规则 | 不重复已有先手技能、不刷新整场技能循环、不发库存牌、不假弹hello或swim，不改变已认证状态 |

规则1机械出处是 U4 素材 L6 `swim {shield:15}`；规则2采用已提供 U1样板的 `skillOpeningCooldownMs=0`适配合同，只更换符合文具课堂主题的母语标题，不声称零接线或已在当前版本跑通。两条没有新的战斗技能核。

A1 `newConcept=null`是已见组合与已见招的回顾；A2 `newConcept=classroom-bell`是新规则。`boss.notSelected`说明另一条属于另一区，不能随机叠加。头目只在棋盘外主持，圆、亮、滑稽；词的图、字母、颜色和轮廓保留，伤害打墨层，碎层后进入该词真实救援题。伙伴本体不受伤、倒下或死亡。

## 6. 起步、救援目标池与供应

| 区 | 起步使用权与首手示例 | 引擎选词约束 | 绿叶与救后保底 |
|---|---|---|---|
| A1／9词 | **climb、kick、dance、swim**，3棋子＋1效果 | 在本区8枚必会棋子中按现有引擎挑，至少一枚真实 damage、一枚 guard和一个可用助手；效果池仅swim | playground免题场景权；新救词下场保证，jump/slide另给playground选择 |
| A2／8词 | **pencil case、pencil、notebook**，3棋子 | 至少有pencil case护盾、一枚damage和第三位可用助手，优先notebook；本课无效果，`effectCount=0` | classroom免题场景权；新救词下场保证，pencil case/scissors另给classroom选择 |

起步送 rescued使用权与首站各一份手牌，不写最高独立答对或“已学会”。旧词按真实记录带入；已会词打完一场休息一场，不再拿学习赐福，未会的本单元必会词可继续使用。每场清战斗实体，使用权、学习阶段、HP、休息表与组合体验记录分别保存。

每区全部必会词始终保留合法救援入口，包括起步已救未会的复习机会。优先到期逃词，再安排未救词和已救未会词；按目标反查合法完整配方：例如只剩 ski，排 A1-S1-ski；只剩 glue stick，先核 notebook相邻回血前置，再排 A2-S5-glue-stick。不能拿其他场景的题替换成这个词，也不能为了多开岔路重复三个相同焦点。尚未掌握或错过的词继续在同区可达，不用赶七战而强制一次答完。

每赢一场沿 v3给救援本身与按需奖励；路边商店／宝箱初值约每3–4战一次，不常驻。能量按区学习目标配，达成后加目标，不因孩子学得快减能量。答错不扣HP，只有战斗输才按原规则扣；HP归零从当前区重来，已经学会的词保留。只用开区前已会棋子不能通关的底线沿用，但具体敌方等级与预算要在可玩版验证，本稿不捏造胜率或数值达标。

## 7. 每区一局示例种子：内容验收夹具，不是实录

两份例子都使用样板C骨架 `[1,1,3,2,1,2,1]`。A1 seed=84002，A2 seed=84005，均 `seed%3=2`。每区起始 observed为空；示例里有两个词已经积累到可出最高独立档，但还没有认证，只有实际首答记录支持时才能新增学会。学习目标示例N=2，达到后沿v3继续加目标；不是学会两个词就算清完本课。

### 7.1 A1／seed 84002

| 战斗／节点 | 对手配方与唯一新内容 | 真实救援／供应 | 间隔与前置 |
|---|---|---|---|
| 1／0-0 | A1-S1 climb/kick，2枚；新：护同列后排 | 焦点climb，假设真实最高独立档答对，新增学会；下场保证climb | 记climb-rear，不因起步有swim就认为看过敌方swim |
| 2／1-0 | A1-S2 jump/playground，2枚；新：地点羁绊 | 焦点jump答错，逃走但不扣HP；不获jump使用权 | 已实际看到羁绊，记jump-playground；安排第5战回归 |
| 3／2-2 | A1-S4 catch/kick，2枚；新：相邻最低HP护盾 | 救catch，答对未必学会；下场保证catch | 第一个间隔战；另一路2-1为slide/playground，没选不算见过 |
| 补给／第3战后 | 路边商店，不是战斗 | 只按合法已救词及白名单供应，题仍考自身词 | 不推进jump的战斗间隔 |
| 4／3-0 | A1-S1-swim，climb/kick，2枚；新：swim开场盾 | 焦点swim，假设真实最高独立档答对，新增学会；下场保证swim | 第二个间隔战；只有盾真实应用才记card-swim |
| 5／4-0 | 原A1-S2，2枚；零新项 | 同词jump答对，救回未必学会；下场保底jump并给playground选择 | 恰好错后隔两战；不趁重遇附加swim |
| 6／5-1 | A1-S5 dance/kick，2枚；新：邻接加速 | 焦点dance答对救回；下场保证dance | 为boss所需的climb／jump／swim三项前置均在实际已选路径上 |
| 7／6-0 | A1-S7，4枚；复用两组与swim，零新机制 | 胜后以jump作真实复习焦点，不考playground或头目名字 | 墨水怪跑掉；未救fly/ski/slide等仍在本区续段可达 |

### 7.2 A2／seed 84005

| 战斗／节点 | 对手配方与唯一新内容 | 真实救援／供应 | 间隔与前置 |
|---|---|---|---|
| 1／0-0 | A2-S1 pencil case/pencil，2枚；新：护邻居 | 焦点pencil case，假设真实最高独立档答对；下场保底本词 | 记pencil-case-guard，A1词的已会不替代此处演示 |
| 2／1-0 | A2-S4 scissors/classroom，2枚；新：课本羁绊 | 焦点scissors答错，不扣HP、不获使用权 | 已真实显示羁绊；安排第5战同词回归 |
| 3／2-2 | A2-S2 notebook/pencil，2枚；新：相邻最低HP回血 | 救notebook，答对未必学会；下场保底notebook | 必须有真实HP恢复才记notebook-adjacent-heal，否则后续首教前置不能伪通过 |
| 补给／第3战后 | 路边商店，不是战斗 | 合法已救词＋classroom白名单；不解锁playground或A1未学词 | 不推进scissors间隔 |
| 4／3-0 | A2-S5 eraser/notebook，2枚；新：eraser前排daze | 焦点eraser，假设真实最高独立档答对；下场保底eraser | notebook回血必须已见；不把控制与回血说成新数值连锁 |
| 5／4-0 | 原A2-S4，2枚；零新项 | 同词scissors答对救回；下场保底本词并给classroom选择 | 空过第3/4战，原配方复现，没有新增eraser或铃声 |
| 6／5-1 | A2-S3 pencil case/classroom，2枚；新：第二条地点羁绊 | 焦点pencil case按真实阶段复习 | 另一路5-0可选A2-S6 ruler/notebook，没选不算见过ruler招 |
| 7／6-0 | A2-S7，4枚；唯一新规则：课堂铃声 | 胜后救／复习pencil；不考classroom和环境 | 笔袋护铅笔与剪刀/教室均已见；两组隔列，未偷偷把第6战的另一bond叠进去 |

若第3战 notebook始终没有发生有效回血，这份“正常演示成功”的固定夹具就不适用：调度器必须先补合法S2展示，推迟依赖它的S5；已有逃词第5战的承诺仍优先，不可为了凑夹具把未发生效果标成已发生。若jump/scissors在第5战仍答错，以b=5重新排第6/7战间隔，第8战回同词续段。头目结束与跨区入口不能吞掉这条队列。

## 8. 交付检查、薄弱处理与未测边界

### 8.1 素材薄弱项怎样处理

| 素材提示 | 本稿处理 | 明确不做 |
|---|---|---|
| kick／playground机械相同 | kick用于必会动作主体；playground作为免题场所，让jump、slide的真实地点羁绊成立。S1先看kick原输出，S2/S3再看地点组合 | 不宣称kick比playground踢得快、远或更强；不把playground做新题 |
| ruler／classroom机械相同 | ruler独立有自己的图词音与真实题；classroom作为免题场所启用笔袋／剪刀现有bond。ruler变体由单词覆盖需求调度 | 不给ruler临时加射程、不把classroom改成全队buff；不靠多加技能区分同效词 |
| 目标条件和控制收益不能从素材推出 | 每次说明真实target；notebook/catch严格最低当前HP；控制词只教原控制 | 不保证“控住后必杀”，不从description脑补引擎回退、持续或动画 |

内容结构自检覆盖：两区required集合9＋8＝17且无重叠；六类＋头目配方齐全；每个必会词至少一个合法救援入口；effect不占棋位；两区4个完整变体独立替换；所有棋位合法不重叠；四条引用bond实际相邻；头目分组没有额外第三bond；两份seed与骨架一致、前置顺序可达、错后两战第5战回归；JSON区对象顶层与固定嵌套字段和样板一致。机械数值来自素材，没有手写新战斗数值。

程序接入后应能抓住的反例：

- 把playground/classroom或其他未授权词出成题；将U1 ball、U3 orange或A1未学词当作A2绿叶；任一项出现即失败。
- climb护斜后方、catch/notebook选全队最低或选自己、满血假回血仍标observed、dance影响斜角、敌方效果改写玩家学习记录，均失败。
- 没先看到笔袋护邻居就首教笔袋/教室；S1-swim与S1-ski同时叠；把三个控制词塞进一场首教，均失败。
- A1泳前规则给全体15盾或执行两遍；A2铃声也附带swim，已有先手再多执行一次；笔袋与头目教室意外相邻触发第三bond，均失败。
- 读档重放敌方用招；第3/4战拿逃词做救援焦点；把补给当完整间隔战；头目后丢掉逃词，均失败。
- 被动看见招式就算学会、绿叶计入本区分母、只用旧词即可通关却没有被体验验收发现，均失败。

| 用户要求 | 本稿回应 |
|---|---|
| 按课分区，写理由 | 开头：L6/L7各一区，9＋8词；两区各有独立课本地点组合 |
| 六类场景、单招变体、头目与示例种子 | §4每区六类＋S7、独立变体；§5两规则；§7两种子 |
| 谁帮谁必须真实，正交相邻 | §4.5逐种target、条件；JSON明确棋位；头目隔列避免隐含bond |
| 只考必会词，绿叶显式白名单 | §2全17词；§3 A1只playground、A2只classroom；所有供应入口同边界 |
| 效果牌是真实对面自己用的招 | §4 swim动作时机、§5盾15；原牌不站棋盘，环境不伪造卡来源 |
| 不新写英文句子 | §2引用核验，§3仅四条已给羁绊语块；approvedContextRefs留空 |
| 薄弱项必须有处理 | §8.1两组同效词按必会主体与免题场所区分，不编效果 |

未做实战：没有运行游戏源码、浏览器、数值模拟或孩子试玩；没有确认当前程序已接上全部事件。上面是可检查的内容与数据合同，不能代替手机体验验收。尤其真实回血触发、后排目标不存在时的现有回退、swim来源事件、头目先手队列与旧词通关底线须在可玩版核验；这不阻挡先交内容表。

