---
title: ScEffect
createTime: 2025/07/07 17:43:56
permalink: /wiki/wovxwpae/
---
#### Overall

一般情况下，一个 ScEffect 的词条具有如下形式：

```json
"<namespace>:<effect_type>-<name>": {
    <arg1>:<val1>,
    <arg2>:<val2>
  }
```

**如何理解**<kbd>**"\<namespace>:\<effect_type>-\<name>"**</kbd>**&#x20;？**

从整体看，它是一个 MC 命名空间 ID，其中，模组对命名空间(namespace)不做任何要求，除非有特殊需求，**您可以安全的将它们简单的简化为**<kbd>**"\<effect_type>-\<name>"**</kbd>**&#x20;（即默认命名空间**<kbd>**minecraft**</kbd>**）**

<kbd>effect_type</kbd>因 ScEffect 种类而异（每种 ScEffect 可能有多个可供识别的词条，例如[修改实体属性](#修改实体属性attribute)<kbd>efc.attr</kbd>或<kbd>attr</kbd>）模组依赖该部分识别该词条为哪一种 ScEffect，请务必妥善填写，**尤其是当本模组尚未实装错误检查时**

<kbd>name</kbd> 主要用于在 set 中区分词条，未来当实装了模板继承或词条覆盖等功能时可能会有比较重要的作用，目前您只需要用 1，a 等简单数据区分词条即可

接下来，我们将对不同种类的 ScEffect 进行介绍。

截至 0.1.0-alpha，模组支持如下 ScEffect 类型

- [修改实体属性(Attribute)](#修改实体属性attribute)
- [追加生物状态效果(Mob Effects)](#追加生物状态效果mob-effects)
- [移除生物目标选择(targetGoal)](#移除生物目标选择targetgoal)
- [添加生物目标选择(targetGoal)](#添加生物目标选择targetgoal)
- [添加常规 AiGoal](#添加常规-aigoal)
- [添加生物近战 AiGoal(Melee Attack)](#添加生物近战-aigoalmelee-attack)

### 修改实体属性(Attribute)

**effect_type：efc.attr / attr**

```json
{
    "attr": <ResourceLocation>,
    "op": <enum.string>,
    "val": <float>
}
```

- attr：一个指向特定 Atttribue 的 MC 命名空间 ID
- op：一个字符串，描述修正属性的应用乘区，应当且仅应当提供如下数个字段其一：
  - ADDITION：直接加算
  - MULTIPLY_BASE：直接乘算
  - MULTIPLY_TOTAL：最终乘算
- val：一个浮点数，描述修正值

### 追加生物状态效果(Mob Effects)

**effect_type：efc.effect / effc**

```json
{
  "effectID": <ResourceLocation>,
  "level": <int>,
  "duration": <int>,
  "visible": <bool>
}
```

- effectID：一个指向特定状态效果的命名空间 ID
- level：状态效果等级，**注意：该值并非游戏内保存的 amplifier（倍率），而是直接的显示等级，即力量 II 中，此处应当设置 2**
- duration：一个浮点数，描述效果持续的游戏刻，**-1 表示永续**
- visible：一个布尔值，控制状态效果的粒子是否显示

### 移除生物目标选择(targetGoal)

**effect_type：goal.target.rm / tarrm**

**未来会提供更细颗粒度的移除**

```json
{
  "hurt": <bool>,
  "nearest": <bool>
}
```

- hurt：一个布尔值，控制是否移除<kbd>HurtByTargetGoal</kbd> 种类的目标选择
- nearest：一个布尔值，控制是否移除<kbd>NearestAttackableTargetGoal</kbd>种类的目标选择

### 添加生物目标选择(targetGoal)

**effect_type：goal.target. add / taradd**

```json
{
  "priority": <int>,
  "target": <ResourceLocation>,
  "rand_interval":<int>,
  "must_see":<bool>,
  "must_reach":<bool>
}
```

- priority：目标选择的优先级，值越小优先级越高
- target：一个命名空间 ID（指向需要被生成的实体的**注册 ID**）或实体标签（ID 前加"#"，同原版标签声明方式）或预定类型；预定类型如下：
  - living：该生物将选择一切生物为目标
  - monster：该生物将选择一切继承自 Monster 的类的生物作为目标
  - player：该生物将选择玩家作为目标
- rand_interval：一个整数，控制触发目标选择的间隔，大部分情况下保留默认值 10 即可
- must_see：一个布尔值，控制是否可在未看见目标的情况下触发目标选择
- must_reach： 一个布尔值，控制是否可在无法到达目标的情况下触发目标选择

### 添加生物近战 AiGoal(Melee Attack)

**effect_type：goal.attack.melee / gatkm**

```json
{
  "priority": <int>,
  "speed":<float>,
  "follow_blind":<bool>
}
```

- priority：目标选择的优先级，值越小优先级越高
- speed：一个浮点数，控制触发近战目标时，实体的移动速度修正
- follow_blind：一个布尔值，控制是否可在无法看见目标的情况下触发近战目标

### 添加常规 AiGoal

**effect_type：goal.common / gcomn**

```json
{
  "priority": <int>,
  "goal":<ResourceLocation>,
  "arg":<CommonGoalArg>
}
```

- priority：目标选择的优先级，值越小优先级越高
- goal：一个命名空间 ID 指向如下 goal 其一（**由于 Goal 在原版不存在注册一说，所以严重依赖手动的配表）：**
  - avoid
  - breath
  - float
  - panic
  - move_restrict
  - moveto_target
  - random_look
- arg：一个 json 块，存储了可能会用到的[goal 配置信息](sceffect.md#commongoalarg)

#### CommonGoalArg

```json
{
    "speed":<float>,
    "range":<float>,
    "entity_type":<ResourceLocation>
}
```
