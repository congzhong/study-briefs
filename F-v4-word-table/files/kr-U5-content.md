# 冒险 v3 U5｜开心·颜色·天气：二区内容表与数据草案

> 配套交付：`KR-U5-冒险数据-2026-10-20.json`按区顺序存放同形区对象数组；如接线器要求与U1一样的单对象根，使用本目录`分区JSON/`中的对应文件。数组仅是多区打包容器，区对象不新增字段。

2026-10-20；依据用户上传的冒险核心 v3、U1 定稿样板和机器素材表制作。**按课分两区：A1=L8（11个必会词），A2=L9＋L10（19个），合计30个，不重不漏。**

理由：L8有5枚必会棋子，能独立组成开心、补给和派对的救援区；L9的9个必会词全是效果／装备，没有本课必会棋子。把L9单独拆一区，会让整区主要靠借绿叶或前区旧棋子撑起战斗。与L10合区后，颜色和天气都能真实用在本区的hat、snowman身上，白色／雪人、黄色／帽子的匹配也能当场复刻。19是持续覆盖目标，不承诺一段七战学完19词；区内可随时停、续走和续救，因此不为凑对称而拆成三个头目区。

本单元恰好两条头目规则：A1“生日添一份”复用birthday的真实经济效果，A2“第一招变成阵雨”复用raining的真实首招效果。都由对面真实使用本单元卡牌产生，不叠加全场环境版本。

数据交付：`分区JSON/KR-U5-A1-冒险数据-2026-10-20.json`、`分区JSON/KR-U5-A2-冒险数据-2026-10-20.json`；每个区记录与U1样板使用同一组字段。单元汇总可顺序容纳这两个完整区记录，不能把两区的SCENES混成一个区，也不能把两个boss对象当作同场叠加规则。新增内容草案，**不导入、不改现有cards/bonds/skills/context-sentences**。

## 1. 已读附件与草案适配边界

本稿实际核对的是上传附件，没有访问游戏仓库或运行时实现。`schemaVersion=1`沿样板表示草案数据版本，不代表当前客户端已经支持。不得把U1样板中“已查分支、提交头”的历史叙述复制为本次验收事实。

| 当前样板入口 | 本草案对应字段 | 程序接入时必须改到 |
|---|---|---|
| 一个区对象 | unit=5，area=1/2，lessons=[8]/[9,10] | 按area选一个完整记录；requiredWords分别11、19，总和30 |
| SCENES与SCENE_HELP空0项，1–7项 | 同名、同形；tuple仍为[card,r,c] | 每区独立索引；只允许实际棋子／场所，不把天气、数词、颜色牌摆成棋子 |
| 基础／变体全量配方 | sceneRules、sceneVariants | 每次二选一；不能叠上同族的五个数字或八个颜色 |
| 效果执行 | enemyActions、deferredSpawnUids | 调用原施牌、商店、购买、部署和合成；前后库存、能量及场上实体都要真实变化 |
| 组合先后 | teachingPolicy、requiresObserved | 只认当前区已经走过且真实展示的战斗；没有前置就先派简单配方，不能靠关卡层数跳过 |
| 单个头目 | boss、S7.enemyActions | boss只是规则元数据；真实施牌只由S7动作执行一次。skillOpeningCooldownMs=null，不能误接成U1铃声或另一次雨天效果 |
| 句子引用 | questionPolicy.approvedContextRefs=[] | 完整句库未提供；接入时仅按同一word键、approved=true和原needs条件解析已有句子，无记录就沿图／音／拼写入口 |

`buy-from-existing-shop-then-deploy`和`buy-from-refreshed-shop-then-deploy`是用现有字段表达的接线合同：调用游戏已有购买与部署流程，不是发明卡牌效果。敌人拥有私有商店、预算、库存与已执行状态；本稿没有声称这些描述已能被当前解析器直接识别。

## 2. 30个必会词：身份、出现方式与已有句子

词、课号、种类、作用与参数依据附件《4-素材-kr-U5.md》L8/L9/L10。L8的11词全部在A1；L9的9词和L10的10词全部在A2。每个词有实际配方里的合法救援入口，起步词也保留后续学习机会。

| 词 | 课／现有kind | 对手如何让孩子看见 | 主要救援场景 | 套手册条款 |
|---|---|---|---|---|
| angry | L8 object | 原前排damage24；受happy加速，也能受cake回血 | A1-S1／S2／S3／S7 | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| birthday | L8 effect | 对敌私有商店加4能量，再真实买已知同词牌；本牌不站场 | A1-S1-birthday／S7 | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| eight | L8 effect | 只取1张angry同词牌并走原合成 | A1-S1-eight | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| happy | L8 object | 邻接加速；见过后再教happy/party原羁绊 | A1-S1／S4／S5／S6／S7 | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| hungry | L8 object | 用与angry相同的前排damage24，不伪造强弱差异 | A1-S1-hungry | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| nine | L8 effect | 只取1张angry同词牌并走原合成 | A1-S1-nine | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| sad | L8 object | 单独展示原enemy-front/daze；happy的加速已教 | A1-S5 | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| seven | L8 effect | 只取1张angry同词牌并走原合成 | A1-S1-seven | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| six | L8 effect | 先教这一张的同词取牌与合成；作为生日购买同词牌的动作前置 | A1-S1-six | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| ten | L8 effect | 只取1张angry同词牌并走原合成 | A1-S1-ten | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| tired | L8 object | 单独展示原enemy-neighbours/slowNext | A1-S6 | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| blue | L9 equipment | 给hat装备，只演通用上限×1.1，不借crayon | A2-S1-blue | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| brown | L9 equipment | 给hat装备，只演通用上限×1.1，不借dog/bread | A2-S1-brown | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| color | L9 effect | 从hat/snowman/umbrella三种合法棋子中真选umbrella，延迟部署一次 | A2-S5 | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| green | L9 equipment | 给hat装备，只演通用上限×1.1；salad是U3必会，不能借绿叶 | A2-S1-green | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| pink | L9 equipment | 给hat装备，只演通用上限×1.1；不借pig | A2-S1-pink | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| purple | L9 equipment | 给hat装备，只演通用上限×1.1；grapes/fig均是U3必会，不能借绿叶 | A2-S1-purple | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| red | L9 equipment | 给hat装备，只演通用上限×1.1；不借apple/tomato | A2-S1-red | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| white | L9 equipment | 给本区snowman装备，命中原matches；hpFactor/matchFactor的组合沿原实现，不承诺最终倍率 | A2-S3-white | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| yellow | L9 equipment | 给本区hat装备，命中原matches；hpFactor/matchFactor的组合沿原实现，不承诺最终倍率 | A2-S1-yellow | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| cloudy | L10 effect | 仅hat自己挡首个符合原判定的攻击1次 | A2-S1-cloudy | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| cold | L10 equipment | 仅hat开战护盾15；不是邻接护盾或闪避 | A2-S1-cold | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| hat | L10 object | 原前排daze；邻umbrella受护，再见beach羁绊 | A2-S1／S2／S4／S5／S6／S7 | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| hot | L10 effect | 仅hat按原rate=1.5出手更勤；不改播放速度 | A2-S1-hot | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| raining | L10 effect | 仅beach下场第1招按原全体目标、factor=0.5执行 | A2-S2-raining／S7 | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| snowing | L10 effect | hat自己及正交相邻umbrella各挡1次；斜角不算 | A2-S1-snowing | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| snowman | L10 object | 原enemy-front/slowNext；受umbrella邻接护盾 | A2-S3／S7 | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| sunny | L10 equipment | 仅beach第一招factor=1.5，原前排目标不变 | A2-S2-sunny | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| weather | L10 effect | 真刷新敌私有商店，原highestUnlocked限定在合法池；真买umbrella并部署 | A2-S6 | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |
| windy | L10 equipment | 只将beach普攻basicTarget改成enemy-neighbours，技能不改 | A2-S2-windy | 沿样板题词／动作／救援口径；本稿§4、§6、§8 |

**句子处理**：本稿不写新英文句子，也不将表内羁绊语块扩写成问答句。仅在§3原样列出素材已有的语块及页码；这些羁绊记录不等于已批准的本牌句。`approvedContextRefs=[]`表示附件不足以确认哪些本牌句已批准，不能理解为“本书没有句子”。读音、卡图、现有题型继续按该词自己的ID使用，不能因两个词效果相同而换成另一个词的题。

## 3. 绿叶撑场景：分区白名单与真实羁绊

### A1白名单：cake、party

| 绿叶 | 课本／游戏归属 | 为什么搭在一起更强（真实机制） | 只引用已有语块及出处 | 在本区怎样给孩子用 |
|---|---|---|---|---|
| cake | U5 L8，非必会object | 给上下左右队友回血8；例如B3的cake给F3的angry回墨层对应的战斗HP | `cake at the party`；素材记录bonds.json cake/party，p97、p103插图 | 开区给免题场景使用权；正常绿叶位可买，不占11词进度。相邻回血先教，再引入party羁绊 |
| party | U5 L8，非必会place | 原enemy-neighbours/damage10；与cake或happy相邻时走各自已有bond，数值不自编 | `happy at the party`：p97、p103（素材注明既有原句）；`cake at the party`：p97、p103插图 | 可在敌我两边实际上场；救回happy后下一场保证party能选择，不能挤掉本词保底 |

A1-S3的cake＋party都是绿叶，因此增加已经见过的angry作为第三枚合法救援对象。它站在F1，与F4/B4的绿叶组合隔开；不为让纯绿叶羁绊可考而把cake或party改成必会词，也不把angry的题冒充羁绊题。

### A2白名单：umbrella、beach、cake

| 绿叶 | 课本／游戏归属 | 为什么搭在一起更强（真实机制） | 只引用已有语块及出处 | 在本区怎样给孩子用 |
|---|---|---|---|---|
| umbrella | U5 L10，非必会object | ally-lowest/guard15：只在上下左右邻居中选当前HP最低者；两枚配方里hat或snowman是唯一邻居 | 无新增羁绊句；这是素材“实际可成立的帮助配对” | 首站可在合法绿叶位选择，免题；不是全队最低血保护，不保护斜角 |
| beach | U5 L10，非必会place | 原enemy-front/damage16；与hat正交相邻触发原bond，给本区两个控制型必会棋子补一个真实输出搭档 | `hat on the beach`；素材bonds.json hat/beach，p120插图 | 首站保证有可购买选择；救hat后继续配对供应。作为天气演示承受者时仍是绿叶，本牌不考题 |
| cake | U5 L8，非必会object，本单元跨课借用 | ally-neighbours/heal8；在A2只单教给hat回血，或作为孩子可选搭档 | 不带party入A2，因而本区不显示cake/party语块或羁绊 | 本区重新明确授权；不因A1见过就推断本区孩子已经展示过其技能 |

这些都是真棋子／场所，计入2–3枚和最多4枚的上限。买、出、领奖、合成及任何羁绊入口均不考绿叶，也不偷偷换考同场必会词。绿叶使用权免费，正常商店购买仍按原游戏能量价格；本稿不改变单元商业解锁。

**不得借入的匹配端点**：salad、fig、grapes、apple、bread等在U3是必会词；pig、crayon、dog等也不能因为颜色牌想配它们就改成绿叶。本稿两个区都不依赖任何跨单元未学必会词。U5素材的zebra/zoo羁绊需要U2必会zebra，本稿也不启用，zoo不进白名单。颜色通用分支详见§8。

## 4. 六类小场景＋墨水怪：一次只教一个新东西

坐标严格沿样板：r=0为前排F，r=1为后排B，c=0–4，对应F1–F5/B1–B5；相邻是上下左右，曼哈顿距离1。tuple顺序就是uid顺序。两个区分别有六类基础小场景＋S7头目；基础族不是强迫每段一场不漏。所有原技能继续生效，不能通过关闭绿叶技能来伪装成只有一种新东西。

### 4.1 A1六类基础配方＋头目

| 场景／类型 | 最终敌阵 | 唯一新东西 | 必须在已走路径中见过 | 实际展示与救援候选 |
|---|---|---|---|---|
| **A1-S1 开心带动重拳**／普通 | angry F3、happy B3（2枚） | `happy-haste` | 无，先从它开始 | happy加速上下左右的angry；斜角无效。angry照原技能打前排，不新增攻击倍率。 可救：angry、happy。 |
| **A1-S2 蛋糕递给前排**／普通 | angry F3、cake B3（2枚） | `cake-heal` | happy-haste | cake按原技能给正前方angry回血8；须真实产生回血才记已展示，angry攻击是复习。 可救：angry。 |
| **A1-S3 蛋糕来到派对**／普通 | angry F1、party F4、cake B4（3枚） | `cake-party` | happy-haste、cake-heal | 右侧只新增cake与party的已有羁绊，cake邻接回血已见；左侧angry与右侧隔两列，不添新帮助关系。 可救：angry。 |
| **A1-S4 派对上的开心伙伴**／普通 | party F3、happy B3（2枚） | `happy-party` | happy-haste | 只新增happy与party的已有羁绊；happy给正前方party加速沿已教的邻接机制。 可救：happy。 |
| **A1-S5 哭声让前排愣一下**／普通 | sad F3、happy B3（2枚） | `sad-daze` | happy-haste | 只新教sad对敌方前排的原daze；happy邻接加速是复习，不声称控制必定带来额外伤害。 可救：sad、happy。 |
| **A1-S6 困意拖慢一片**／普通 | tired F3、happy B3（2枚） | `tired-slow` | happy-haste | 只新教tired对敌方一片的原slowNext；happy仍只按邻接加速，不改为全队技能。 可救：tired、happy。 |
| **A1-S7 淘气墨水怪：生日添一份**／头目 | angry F1、happy B1、party F4、cake B4（4枚） | `card-birthday` | happy-haste、cake-heal、cake-party、card-six | 只新增birthday让敌私有商店多4能量，再按真实价格买已在架的angry同词牌并原路合成；两侧已教组合不相邻。 可救：angry、happy、birthday。 |

A1的教学顺序先教happy邻接加速，再教cake邻接回血；两条party羁绊各自单独出现。sad和tired各只引入自己的控制／减速，不声称“控住后angry必定打出更高伤害”。素材明确未证明这种目标重叠和实战收益，本稿不拿它们代替真实帮助配对。

### 4.2 A1单招／单牌变体：完整配方二选一

| 变体 | 敌阵／唯一新增动作 | 已见前置 | 本场可救词 |
|---|---|---|---|
| `U5-A1-S1-six` | angry F3、happy B3（2枚）；只新增six这张牌：从敌私有牌库取1张angry进手牌，再真实部署合成；数词不同不改变copies=1。 | happy-haste | angry、happy、six |
| `U5-A1-S1-seven` | angry F3、happy B3（2枚）；只新增seven这张牌：从敌私有牌库取1张angry进手牌，再真实部署合成；数词不同不改变copies=1。 | happy-haste | angry、happy、seven |
| `U5-A1-S1-eight` | angry F3、happy B3（2枚）；只新增eight这张牌：从敌私有牌库取1张angry进手牌，再真实部署合成；数词不同不改变copies=1。 | happy-haste | angry、happy、eight |
| `U5-A1-S1-nine` | angry F3、happy B3（2枚）；只新增nine这张牌：从敌私有牌库取1张angry进手牌，再真实部署合成；数词不同不改变copies=1。 | happy-haste | angry、happy、nine |
| `U5-A1-S1-ten` | angry F3、happy B3（2枚）；只新增ten这张牌：从敌私有牌库取1张angry进手牌，再真实部署合成；数词不同不改变copies=1。 | happy-haste | angry、happy、ten |
| `U5-A1-S1-hungry` | hungry F3、happy B3（2枚）；只替换成hungry的原重击；其55血、7000ms、前排damage24与angry相同，不宣称谁更强。happy邻接加速是复习。 | happy-haste | hungry、happy |
| `U5-A1-S1-birthday` | angry F3、happy B3（2枚）；敌私有商店真实加4能量，再用原购买和部署流程拿已上架angry并合成；不是生日直接升级。 | happy-haste、card-six | angry、happy、birthday |

数字变体统一使用angry作目标，刻意控制其他变化。six/seven/eight/nine/ten每一张都是copies=1，绝不解释成得到6–10张牌。取到的复制牌先真实进敌手，再合回原angry；没有第二个场上angry，也不直接写觉醒。birthday的小场景排在six已经演示之后，避免第一次同时教“商店加钱”和“同词合成”。

### 4.3 A2六类基础配方＋头目

| 场景／类型 | 最终敌阵 | 唯一新东西 | 必须在已走路径中见过 | 实际展示与救援候选 |
|---|---|---|---|---|
| **A2-S1 雨伞护着帽子**／普通 | hat F3、umbrella B3（2枚） | `umbrella-guard` | 无，先从它开始 | umbrella只给上下左右中当前血量最低的伙伴护盾15；此阵唯一邻居是hat，斜角不算。hat照原技能控前排。 可救：hat。 |
| **A2-S2 帽子来到海边**／普通 | beach F3、hat B3（2枚） | `hat-beach` | umbrella-guard | 只新增hat邻beach的已有羁绊；海滩仍用原damage16打前排，不编造帽子的额外伤害。 可救：hat。 |
| **A2-S3 雪人扔出慢雪球**／普通 | snowman F3、umbrella B3（2枚） | `snowman-slow` | umbrella-guard | 只新教snowman对敌方前排的原slowNext；umbrella只保护唯一相邻snowman的机制是复习。 可救：snowman。 |
| **A2-S4 蛋糕分给帽子**／普通 | hat F3、cake B3（2枚） | `cake-hat-heal` | umbrella-guard | 只新增cake给相邻hat回血8；实际回血前不登记演示，hat的原控制沿用。 可救：hat。 |
| **A2-S5 挑一把已经认识的雨伞**／精英 | hat F3、umbrella B3（2枚） | `card-color` | umbrella-guard、snowman-slow | color真亮出hat、snowman、umbrella三种合法棋子并挑umbrella，再只创建一次；雨伞是本区白名单绿叶，免题。只救实际出现的hat或color。 可救：hat、color。 |
| **A2-S6 天气一变重看商店**／精英 | hat F3、umbrella B3（2枚） | `card-weather` | umbrella-guard、card-color | weather真刷新敌私有商店；firstSlot=highestUnlocked在本区授权池内由原实现解析。合法新架实际有umbrella且能支付才买并真实部署到预留B3；不把刷新写成加速或天气伤害。 可救：hat、weather。 |
| **A2-S7 淘气墨水怪：第一招变成阵雨**／头目 | beach F1、hat B1、snowman F4、umbrella B4（4枚） | `card-raining` | umbrella-guard、hat-beach、snowman-slow | 只给enemy beach实际施raining：它的下场第1招改为原敌方全体范围，伤害按factor=0.5；只这枚一次。两对已教组合隔两列，没有全场环境加成。 可救：hat、snowman、raining。 |

A2-S5的三个候选是hat、snowman、umbrella，均须已在实际所选战斗见过。umbrella是本区明确授权的绿叶，挑中后无需绿叶题；仍只在战后考选定的color或实际在场hat。U1样板的is配方不含绿叶是那份具体配方的约束，本区没有把未授权必会词混进color菜单。

### 4.4 A2单招／单装备变体：完整配方二选一

| 变体 | 敌阵／唯一新增动作 | 已见前置 | 本场可救词 |
|---|---|---|---|
| `U5-A2-S1-blue` | hat F3、umbrella B3（2枚）；只新增blue装备hat，×1.1通用；crayon不在本区，不能冒用匹配；护盾搭档是复习，其他颜色不叠入本配方。 | umbrella-guard | hat、blue |
| `U5-A2-S1-brown` | hat F3、umbrella B3（2枚）；只新增brown装备hat，×1.1通用；dog/bread均不是合法绿叶；护盾搭档是复习，其他颜色不叠入本配方。 | umbrella-guard | hat、brown |
| `U5-A2-S1-green` | hat F3、umbrella B3（2枚）；只新增green装备hat，×1.1通用；salad是U3必会词，不借；护盾搭档是复习，其他颜色不叠入本配方。 | umbrella-guard | hat、green |
| `U5-A2-S1-pink` | hat F3、umbrella B3（2枚）；只新增pink装备hat，×1.1通用；pig是U1必会词，不借；护盾搭档是复习，其他颜色不叠入本配方。 | umbrella-guard | hat、pink |
| `U5-A2-S1-purple` | hat F3、umbrella B3（2枚）；只新增purple装备hat，×1.1通用；grapes/fig都是U3必会词，不借；护盾搭档是复习，其他颜色不叠入本配方。 | umbrella-guard | hat、purple |
| `U5-A2-S1-red` | hat F3、umbrella B3（2枚）；只新增red装备hat，×1.1通用；apple/tomato均不借作绿叶；护盾搭档是复习，其他颜色不叠入本配方。 | umbrella-guard | hat、red |
| `U5-A2-S3-white` | snowman F3、umbrella B3（2枚）；只新增white装备snowman，命中原matches中的snowman；hpFactor=1.1与matchFactor=1.25如何选用或组合完全调用原装备实现，本稿不手乘、不承诺最终倍率；护盾搭档是复习，其他颜色不叠入本配方。 | snowman-slow | snowman、white |
| `U5-A2-S1-yellow` | hat F3、umbrella B3（2枚）；只新增yellow装备hat，命中原matches中的hat；hpFactor=1.1与matchFactor=1.25如何选用或组合完全调用原装备实现，本稿不手乘、不承诺最终倍率；护盾搭档是复习，其他颜色不叠入本配方。 | umbrella-guard | hat、yellow |
| `U5-A2-S1-cloudy` | hat F3、umbrella B3（2枚）；cloudy只给hat自己挡首个符合判定的攻击1次，umbrella不获得挡次；不与同场其他新天气牌或颜色装备叠加。 | umbrella-guard | hat、cloudy |
| `U5-A2-S1-cold` | hat F3、umbrella B3（2枚）；cold只给hat开战护盾15，不给邻居加盾，也不把护盾写成躲避；不与同场其他新天气牌或颜色装备叠加。 | umbrella-guard | hat、cold |
| `U5-A2-S1-hot` | hat F3、umbrella B3（2枚）；hot只让hat按原rate=1.5出手更勤，不把游戏播放速度变快；不与同场其他新天气牌或颜色装备叠加。 | umbrella-guard | hat、hot |
| `U5-A2-S1-snowing` | hat F3、umbrella B3（2枚）；snowing给hat自己及上下左右相邻的umbrella各挡1次；只覆盖施放时原规则的合法相邻对象，不算斜角；不与同场其他新天气牌或颜色装备叠加。 | umbrella-guard | hat、snowing |
| `U5-A2-S2-raining` | beach F3、hat B3（2枚）；只给beach的第1招用原全体范围、伤害factor=0.5，之后恢复原技能目标；不改双方全队；hat/beach羁绊是复习。 | hat-beach | hat、raining |
| `U5-A2-S2-sunny` | beach F3、hat B3（2枚）；只给beach装备sunny，第一招按factor=1.5，原enemy-front目标不变；hat/beach羁绊是复习。 | hat-beach | hat、sunny |
| `U5-A2-S2-windy` | beach F3、hat B3（2枚）；只给beach装备windy，把普攻basicTarget改为enemy-neighbours；技能仍沿原规则，不写成技能打一片；hat/beach羁绊是复习。 | hat-beach | hat、windy |

每一行只有这一张颜色或天气牌，不把blue、yellow、cold等依次叠上一个“展示人偶”。即使它们共用同一副基础阵容，也各自展示自己的真实来源牌、图片和题词。

同效词的战斗变化有限，不能把换数字或换颜色算成已经解决新鲜感。内容调度按未救词队列轮换这些变体，并与已满足前置的控制、天气、其他组合或间隔复习穿插；不连续铺出五个数字或六个通用颜色的首教。到期逃词重遇优先于穿插安排，不能为了换花样推迟已承诺的本词复习。

### 4.5 排序、效果动作与44px弹图

- **先组合、再单招、最后拼已教组合**。前置只记已选并完成的战斗里真实发生的效果。看了地图影子、看了未选支线、救回词、答对最高档，都不自动补教学记录。技能未触发就不登记；后续仍按首教处理。同区续救保留记录，另一分区的记录不能凭空冒充本区已见。
- `sceneVariants`一行就是完整配方；只选基础或一个变体，不能把数组全部追加。逃词复习复现原配方，零新招；可重复已经见过的场景，但不借机加颜色或天气。
- **第b战救援答错，第b+1、b+2战不以同词作救援目标，第b+3战原配方重遇。**补给、开宝箱界面、看地图、同战多次答题不算两战。期限到了就优先排复习，必要时续接区段，不能因头目到了而丢掉逃词。
- 每场先选焦点必会词，再反查包含它的完整配方；焦点必须出自该次`controlledWords`和`rescueCandidates`，效果牌还必须实际成功应用。普通基础S1不能救没用过的yellow，color未选的候选不进救援池。尾部剩余单词按有限待救队列逐个获得合法入口，不能一直被起步两枚棋子挤掉。
- 敌施牌只动敌私有状态，不消耗玩家牌库、不替玩家答题、不写玩家学会标记。素材对效果牌“可演示”标的是候选，本稿给出待接线的真实流程，不能当作现有动画已经全部可用。
- 装备在原装备槽真实应用后弹一次44px本牌图；cloudy在真的挡住一次时弹，cold在开战护盾真的添加时弹，sunny/raining在真实第一招生效时弹，windy在真实受影响的普攻时弹。hot须在原rate状态已落到目标后展示；snowing须真实给目标及正交邻居添加原挡次，再以来源标记关联实际挡次消耗，不把静态弹图算发生了防御。
- birthday的+4写在敌私有商店能量上；它的跟随购买确实落到angry后，才指向正确棋子的复制／合成。color先跳过uid2，真选出umbrella后才部署一次。weather同样先跳过uid2，刷新后实际购买umbrella并放到预留B3；并非在已有雨伞旁再生一把。
- 每次动作结果、已扣库存与延迟创建结果随场缓存；读档不再加4、不再刷新、不再复制、不再合成。地图揭晓后固定承诺，不通过重进节点暗换待救词。保持原战斗速度，44px反馈不顺带触发慢镜。

## 5. 头目规则选择：每单元两条，分别落在两区

| 规则／区域 | 真实作用范围与参数 | 必须已经展示 | 为什么贴合单元、如何看见 | 明确不叠加什么 |
|---|---|---|---|---|
| **U5-birthday-supply：生日添一份／A1** | 敌私有商店施birthday，energy恰好+4；再用原购买流程买架上angry同词牌，原部署／合成接回F1的angry | happy邻接加速、cake邻接回血、cake/party羁绊、six取同词并合成 | 对手在派对前添一份伙伴牌；先看到能量变化，再看到真实购买与合成。四枚最终布局不变，左右两对之间隔两列 | 不按+4直接加血／等级；不将birthday改成免费拿牌；不新增第五枚棋子；不附带环境铃声 |
| **U5-raining-first-skill：第一招变成阵雨／A2** | 只对S7 uid1的beach实际施raining；下场第1招走原全体目标路径，伤害factor=0.5；后续沿原规则 | umbrella保护、hat/beach羁绊、snowman慢雪球 | 对面原本打前排的海滩，第一招变成较轻的一场阵雨；作用来源可追到raining本牌。左侧海边与右侧雨伞雪人两对拼回已见内容 | 不对双方每枚棋子套雨；不每招全体；不加持续伤害或额外减速；不与sunny、windy、cold同时首教 |

**birthday购买的合法性**：沿当前卡牌真实价格p与既有商店库存，确定本模板的固定备战预算E，使E<p且E+4≥p。这样加4确实使那次购买成为可能，不出现有钱却不花、或无货假买。若实际价格／库存无法满足，就在节点揭晓前换到合法的birthday配方，不能假写“已买”。支付p后真实剩余E+4−p；p来自当前原数据，不在内容表另定。购买只调用原buy/deploy/merge，原合成增量由真实实现处理，不能一面预写升级一面又执行合成。

**raining作用的合法性**：beach的原技能是对敌方damage，因此“全体”沿原技能敌我方向转为对方全体；不解释成误伤自己或顺带治疗。factor=0.5作用在原运算路径，不能在高等级时仍硬写固定伤害8。全体目标转换、首招消耗时点和弹图均须程序接入后复核，本稿没有添加新的技能核。

这两条是固定各区规则，不是同场随机抽两条；`boss`只供预览和绑定，实际只执行一次S7中的`enemyActions`。若孩子在前面的单招变体已经见过同一张头目牌，头目按已教组合综合复现，不再声称新增了第二种规则。

墨水怪站棋盘外主持，不占单词位。它圆、亮、滑稽；被困伙伴保留字母、颜色和轮廓，头顶墨滴与汗。伤害打在半透明墨层上，碎层待救；答对真实题后金色旋风与星星入队，词本体不受伤、倒下或消失。不把angry这个情绪词本身画成“邪恶者”。

## 6. 起步、救援目标池与免费供应

### A1：三枚必会棋子＋一张数字效果，共四个起步词

棋子池为angry/happy/hungry/sad/tired。沿学习引擎选易项，并保证happy、一枚实际damage输出angry或hungry、再一枚sad或tired；数字效果在six到ten中选一个。示例：**angry、happy、sad、six**。数字牌必须有合法目标和真实同词库存，不把空卡作为“能用”的首手。

cake/party只给免题场景使用权；正常商店绿叶位能选，操作不考题。happy被救回后，下一场本词保底之外保证party有选择；angry/hungry缺持续支援时可以给cake的合法选择，不因此砍掉未学会必会词的供应。

### A2：两枚必会棋子＋两张可用效果，共四个起步词

本区仅有hat和snowman两枚必会棋子，不能为了硬凑“三枚”偷偷搬来未学的其他单元词。起步给 **hat、snowman、cloudy、hot** 的使用权和首站手牌，沿引擎确定呈现先后；两张效果都有现成承受者。首站合法绿叶位保证beach可选，以其真实输出支撑战斗；本区正常开局能量配额须足以按原价格买到beach，不靠完成别单元才获得，也不要求A1任何必会词已经学会。

umbrella与cake是后续可选支援。hat与snowman虽然技能以控制为主，仍按原战斗实现普攻；本稿未据此假定起步胜率。首屏要有可选beach、买得起且不挤掉必会供应，是这一区接入后必须实际走一遍的检查点。

两区送的都是rescued使用权与首站手牌，**不直接写已学会**。本区赐福只给本单元未学会的必会词；已经学会的词仍照休息规则打完一场休息一场，商店睡觉不可买，没有赐福。每场清棋子实体、按已救集合重新买，不用上一场实体绕过学习。以前真学会的词仍可按核心v3使用，绝不能将它们降格成绿叶或绕过到期题。

新救词下一场保证出现；若已学会且正在休息，保证展示与“睡觉不可买”必须按样板原规则同时保留，不能靠删休息强行保证可买。欠重遇逃词优先，其次尚未救回，再选已救未会；优先词反查合法配方，未满足前置就先补简单场景。初值每3–4场的补给／宝箱仍沿核心v3，隐藏战斗同样从合法场景库取。血量只有战斗失败才扣；救援答错永远不扣血。**起步或此前已经获得使用权的词再次答错时，逃走的是本次待救副本／复习目标；不撤销此前的rescued使用权，也不抹掉已有学习进度或学会记录。**学习模型对本次错误的正常更新仍沿原引擎。

学得快就增加本区待学目标和后续配方，不削能量、不给旧词强行赐福。区结束仍有未救目标时，保留同区续救入口；过了A1头目也不意味着L8十一词自动全学会。纯旧已会阵容不能通关的强度底线沿原冻结强度验证，这份表不拿“起步能打”替代该验收。

## 7. 一局示例种子：每区一段，由浅到深＋间隔复习

两个种子是**确定性内容／流程示例**，不是跑过游戏得到的记录。只固定场景和示例事件；实际题型、是否最高独立档、战斗胜负仍由真实引擎与玩家操作决定。示例中的学会仅由写明的真实最高独立首答正确成立，不能从seed或“答过三次”推导。

### A1：85001，85001 % 3 = 2

骨架采用样板第2号：`[1,1,3,2,1,2,1]`；起步angry/happy/sad/six；初始未认证学会。为演示阶段结算，假设既往学习记录使第4战angry实际获最高独立题。第2战happy错后，完整隔第3、4战，第5战回原S4。

| 第几战／节点 | 这局实际走到的配方 | 焦点词与回答 | 本战新增／复习 | 下一场／后续 |
|---|---|---|---|---|
| 1／`0-0` | `A1-S1` | angry：答对，尚未学会 | `happy-haste` | angry 获下一场保底 |
| 2／`1-0` | `A1-S4` | happy：答错，逃走；不扣血 | `happy-party` | 第3、4战不以此词救援；第5战原配方重遇 |
| 3／`2-2` | `A1-S2` | angry：答对，尚未学会 | `cake-heal` | angry 获下一场保底 |
| 4／`3-0` | `A1-S3` | angry：实际最高独立题首答正确，才记学会 | `cake-party` | angry 获下一场保底 |
| 5／`4-0` | `A1-S4` | happy：答对，尚未学会 | `零新内容，复习 happy-party` | 本战按期重遇；happy 获下一场保底 |
| 6／`5-1` | `U5-A1-S1-six` | six：答对，尚未学会 | `card-six` | six 获下一场保底 |
| 7／`6-0` | `A1-S7` | birthday：答对，尚未学会 | `card-birthday` | birthday 获下一场保底 |

未选支线示例：`2-1`可为A1-S5救sad；没有走就不登记sad-daze。头目之前happy加速、cake回血、cake/party及six真实合成都在实际路径里出现。头目后只将第4战实际最高独立正确的angry记学会；happy/six/birthday是救回，其他词仍在合法待救队列。第3和第4战继续以angry练习不是“换个词替happy复习”，happy的到期重遇始终在第5战。

### A2：85002，85002 % 3 = 0

骨架采用样板第0号：`[1,2,1,2,2,1,1]`；起步hat/snowman/cloudy/hot。示例假设hat有既往真实学习记录，第5战实际独立整词首答正确。第2战hat错后，第3、4战只以snowman、white为救援焦点，第5战回原S2。

| 第几战／节点 | 这局实际走到的配方 | 焦点词与回答 | 本战新增／复习 | 下一场／后续 |
|---|---|---|---|---|
| 1／`0-0` | `A2-S1` | hat：答对，尚未学会 | `umbrella-guard` | hat 获下一场保底 |
| 2／`1-0` | `A2-S2` | hat：答错，逃走；不扣血 | `hat-beach` | 第3、4战不以此词救援；第5战原配方重遇 |
| 3／`2-0` | `A2-S3` | snowman：答对，尚未学会 | `snowman-slow` | snowman 获下一场保底 |
| 4／`3-0` | `U5-A2-S3-white` | white：答对，尚未学会 | `card-white` | white 获下一场保底 |
| 5／`4-0` | `A2-S2` | hat：实际最高独立题首答正确，才记学会 | `零新内容，复习 hat-beach` | 本战按期重遇；hat 获下一场保底 |
| 6／`5-0` | `A2-S5` | color：答对，尚未学会 | `card-color` | color 获下一场保底 |
| 7／`6-0` | `A2-S7` | raining：答对，尚未学会 | `card-raining` | raining 获下一场保底 |

未选支线示例：`3-1`可为基础A2-S5救color，当时umbrella与snowman的前置已经满足；看过但没选不会登记card-color，第6战实际走到S5时仍是第一次展示。第6战color的三个候选都已出现过；只在真实挑中umbrella后创建uid2，绿叶不出题。头目只新增raining，左侧hat/beach、右侧umbrella/snowman都是实际走过的组合。剩余颜色和天气卡继续各自单招救援，不把两区共30词塞进两段14题。

## 8. 交付检查、薄弱处理与未测边界

### 8.1 素材“薄弱”的具体处理

| 薄弱点 | 本稿怎样处理 | 不能误写成什么 |
|---|---|---|
| angry / hungry的机械参数相同 | 55HP、7000ms、enemy-front/damage24按素材保留；hungry只替换词卡与原表现，happy加速作已知背景。分别考自己的词 | hungry比angry伤害高、饥饿会吸血、愤怒会暴击等素材没有的效果 |
| six / seven / eight / nine / ten全是copies=1 | 五个完整单招变体，统一angry作目标以保持其他条件不变；卡图、读音、题词各自独立。词义靠原词卡与真实题学习，不把游戏复制数量当数词词义 | 给6、7、8、9、10张；数越大复制越强；直接觉醒；一次弹五张就视为都学过 |
| cloudy / snowing都写blocks=1 | 按完整description保留目标范围：cloudy只有自身；snowing是自身＋正交邻居。两枚hat/umbrella配方能看出谁得到挡次 | 因单个参数相同就合并为同一个效果，或把snowing写成全场永久免伤 |
| 六种颜色在本区没有合法匹配端点 | blue/brown/green/pink/purple/red只用原通用hpFactor=1.1路径；white/snowman、yellow/hat命中原matches，素材给出的hpFactor=1.1、matchFactor=1.25如何选择或组合完全调用原装备实现。本稿不手工二次乘算，也不承诺最终是1.25或1.375。颜色在题里仍各考各词，不声称八种战斗玩法各不相同 | 把salad、fig当绿叶；偷偷解锁dog、pig等别单元未会词；把两个参数的组合方式未经代码核验就写成结论 |
| L9本身没有必会棋子 | 与L10合区，用hat/snowman承载颜色；当前区起步2棋子＋2效果共4词，beach免题搭档补输出 | 为凑三棋子数把效果牌画成站场人偶，或把绿叶计入本课必会进度 |
| birthday / color / weather在素材标为商店类而非确定战斗演示候选 | 各自只在备战敌私有商店真实执行，加钱／真选／真刷新之后才购买部署；明确库存、预算、延迟创建与读档幂等合同 | 三种牌都套成“让棋子变强”的同一泛光特效，或对玩家实际商店施放 |
| 控制＋输出收益未证实 | 只写sad、tired、hat、snowman的真实target/effect；帮助关系用happy、cake、umbrella等素材可证配对 | 宣称某控制一定帮某输出额外命中、输出一定打中被控目标 |

### 8.2 静态交付核对

两份区JSON保持U1样板完整顶层字段；SCENES和SCENE_HELP保留空0项，各有六基础小场景＋S7。A1七个、A2十五个完整变体，共22个；没有把变体当动作补丁叠加。A1的11词与A2的19词不相交，联合恰好是素材U5的30个必会词；每词均有至少一个完整配方救援入口。绿叶共两区各自的明确白名单，不含跨单元必会词。

坐标、人数、真实目标词、动作目标uid、延迟创建、羁绊两端正交邻接、前置引用、示例节点范围、seed取模和两战间隔均按文件静态检查。头目两对分别放在第1／第4列，避免意外让happy影响另一组或让umbrella在两名新目标间随机改选。所有效果与装备只在enemyActions里，绝无站场tuple。

### 8.3 接入后仍要实测的边界与原话对照

| 用户要求／样板合同 | 这份内容如何响应 | 什么结果算失败 |
|---|---|---|
| “有趣的基础上要尽快且有效率地学会单词” | 每个必会词都有可达配方；按待救词反查、尾词续救；数字和颜色不伪造机制差异 | 连续只出现hat/angry，最后的颜色／数字长期不出；七战结束就把剩余目标丢掉 |
| “每个小场景只教一样新东西” | 单个组合或单张招；前置记真实展示；头目只拼两对已教内容 | 首见happy/party时还同时加五种数字；头目所有前置只来自未选支线 |
| “谁帮谁”必须真的成立 | 邻接按正交；umbrella只有唯一邻居；原目标类型与低血判定写明 | 把斜角画成加成、把umbrella写成全队最低血保护、把控制和输出硬编成固定加伤 |
| 效果必须真实 | 所有商店／装备／天气招调用原路径；真实触发才弹本牌 | 显示+4但预算未加；color部署重复雨伞；raining被当作全队环境；sunny改了普攻 |
| “答错永远不扣血”＋错后隔两战再遇 | 示例第2战错、第5战原配方重遇；期限到可续接区段 | 答错扣血、同一站立即重问、漏掉重遇，或重遇场又加一张新招 |
| “只用以前学会的棋子绝不能通关” | 不给旧已会赐福，照休息规则；对手按独立冻结强度生成 | 只靠旧已会阵容无须新学习就稳定通关；此点不能仅用内容JSON静态检查宣布通过 |

没有游戏运行时源码、实战日志与儿童观察，不能把本稿称为已经完成平衡或体验验收。尤其要实测A2首站两枚控制型必会棋子＋beach供应是否顺畅、生日真实购买和合成是否一次执行、雨天第一招全体及系数是否正确。程序静态检查与限时模拟可以并行；沿核心v3完成一个可玩阶段并由总控完整走一遍，再交手机试玩，模拟不挡交付。
