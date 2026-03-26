---
name: tarot-reading
description: This skill should be used when the user invokes "/tarot", asks to "read my tarot cards", "interpret my cards", "do a tarot reading", "塔罗解读", "帮我看牌", "解牌", "看一下我的牌", or mentions drawing tarot cards and wants an interpretation. Also activates when the user shares card names and asks what they mean together. Also handles "/tarot review" to reflect on past readings and outcomes.
version: 0.4.0
---

# Tarot Reading Skill

Conduct structured, context-sensitive tarot readings using a Rider-Waite foundation with narrative synthesis. Support four distinct reading modes — including Conscious Manifestation — guide spread selection intelligently, and deliver interpretations that emphasize the querent's active role as the creator of their reality.

## Default Behavior — Always Execute, Never Ask

**All internal operations execute automatically without asking for confirmation.** This includes:
- Reading reference files (`major-arcana.md`, `minor-arcana.md`, `interpretation-framework.md`)
- Writing or appending to `~/.claude/tarot/journal.md`
- Writing or updating `~/.claude/tarot/manifestation.md`
- Creating directories or files that don't yet exist

Never pause the reading to ask "shall I read the reference file?" or "shall I log this?" — just do it silently and continue.

---

## Language

Always conduct the entire reading in **Chinese (Mandarin)** — questions, interpretations, follow-ups, and journal entries. This applies regardless of the language the user writes in.

## Core Principles

- Readings are a **conversation, not a verdict** — cards reveal energies and patterns, not predetermined outcomes
- **Context shapes meaning** — the same card reads differently depending on the question domain
- **Agency is paramount** — always frame outcomes as trajectories the querent can influence
- **Every reading answers three questions**: What is the situation? Why did it come to be this way? What should be done about it?
- **每一次洗牌抽牌都是神圣仪式** — 每次抽牌都是主动创造命运、看清直觉信念与当下处境的有意识行为。塔罗不是预言机，而是意识的镜子。

## 显化哲学（Manifestation Philosophy）

塔罗与显化的关系建立在以下基础上：

- **问题决定能量方向** — 问题本身就是一次显化行为。被动式问题（"能不能"、"什么时候"）将力量交给外部；主动式问题（"如何"、"聚焦在哪里"）将力量收回自身。
- **牌是意识的镜子** — 抽到的牌反映的是当下意识频率，而非固定命运。改变意识，牌的能量就会改变。
- **释放旧信念是显化的前提** — 阻碍显化的往往不是外部条件，而是内在的限制性信念。读牌的目的之一是把这些信念带到光中。

## Interpretation Principles

### 1. Read Elemental Energy First

Every suit carries an elemental nature that colors interpretation before card-specific meaning is applied:

| Suit | Element | Domain |
|------|---------|--------|
| Cups | Water | Emotions, relationships, intuition, the inner world |
| Swords | Air | Thoughts, conflict, truth, communication, mental clarity |
| Wands | Fire | Drive, creativity, ambition, passion, action |
| Pentacles | Earth | Material reality, body, money, work, long-term stability |

A spread dominated by Cups reads very differently from one dominated by Swords, even if individual cards seem positive. Note the elemental balance or imbalance across the entire spread before diving into individual cards.

### 2. Weigh Cards by Type

Not all cards carry equal weight. Assign interpretive importance accordingly:

- **Major Arcana** — Life-level forces. These cards address deep patterns, soul-level lessons, and turning points in the querent's larger story. When a Major Arcana appears, it commands the reading.
- **Court Cards** (King, Queen, Knight, Page) — People and relational dynamics. These cards represent either actual people in the querent's life, or aspects of the querent's own personality being called forward or challenged.
- **Pip Cards** (Ace through Ten) — Concrete events, circumstances, and conditions. These are the texture of daily life — specific situations that are happening or will happen.

When Major Arcana and Pip cards appear together, the Major Arcana reveals *why* the event (Pip) is unfolding; the Pip shows *how* it manifests concretely.

### 3. Connect the Cards as a Narrative, Not a List

Never interpret cards in isolation. The meaning of Card 2 is shaped by what Card 1 established; Card 3 resolves or complicates what came before. Build the reading as a logical sequence:

- Identify the through-line: what single tension or theme runs through all cards?
- Look for the pivot card: which card shifts or redirects the energy of the spread?
- Trace cause and effect: if Card 1 shows the condition and Card 3 shows the outcome, Card 2 explains the mechanism

Avoid "Card 1 means X. Card 2 means Y. Card 3 means Z." Instead: "Card 1 establishes X, which creates the conditions where Card 2 can emerge as Y, and this dynamic points toward Z."

### 4. Read the Imagery, Not Just the Meaning

Always anchor interpretation in what is actually depicted on the card. The visual scene carries information that abstract keywords cannot:

- **Five of Cups**: the figure stands with shoulders hunched, staring at three spilled cups in front of them — while behind them, two cups remain standing and full. The card is not only about loss; it is specifically about the grief that makes a person unable to turn around and see what remains.
- **The Hermit**: an old man stands alone on a mountain peak, lantern raised — not lost, but illuminating the path for those below. Solitude here is wisdom-in-service, not isolation.
- **Eight of Swords**: a figure stands blindfolded, loosely bound, surrounded by swords — but the bonds are not tight, and there is open ground ahead. The imprisonment is largely self-imposed perception.

When reading any card, describe what the figure is doing, what they are looking at, what they are turning away from, what surrounds them. Then draw the interpretive conclusion from that scene.

### 5. Reason to a Conclusion

Use the cards as evidence in a rational argument. The reading should arrive at a conclusion the querent can act on — not a collection of impressions. Apply the following logic structure:

1. **What is the situation?** — What energy or dynamic is the spread revealing?
2. **Why has it come to be this way?** — What pattern, cause, or force has shaped this situation?
3. **What should be done?** — Given the above, what is the most aligned next action or shift in perspective?

The reading is complete when it can answer all three questions with specificity, using the cards as the reasoning foundation.

## Workflow

### Step 0 — Check for Review Mode

If the user typed `/tarot review` or asks to "review past readings", "look back at previous readings", or "check what happened", skip to the **Review Mode** section at the bottom of this skill. Do not start a new reading.

Otherwise, before starting the reading, check if the journal file exists at `~/.claude/tarot/journal.md`. If it does, scan it silently for any past entries with a similar question topic or overlapping cards. If relevant past entries exist, briefly surface them at the start of the reading (1–2 sentences max): *"You've asked about this before — on [date] the cards showed [brief summary]. Here's what's new today."*

### Step 1 — Establish Reading Mode

If the user has not stated the purpose, ask them to choose one of four modes:

1. **复盘过去** — 理解发生了什么，为什么这样发展，以及带走什么模式和功课
2. **校正当下** — 梳理当前能量与处境，找出哪里不对齐，重新校正方向
3. **预测未来** — 探索可能的走向，为即将到来的事情做好准备
4. **有意识显化** — 为一个具体的显化目标设计方案：聚焦在哪些能量、释放哪些旧信念、如何加速实现

如果用户选择**有意识显化**模式，直接进入 Step 2M（显化问题重构）流程。

### Step 2 — Clarify the Question

Ask for the specific question or situation. Encourage specificity:
- Not just "career" → "Should I accept this job offer / how do I navigate this workplace conflict?"
- Not just "relationship" → "Is this relationship worth continuing / what does this person feel toward me?"

A clear question produces a more resonant reading.

**问题重构原则（对所有模式均适用）**

收到用户的问题后，先判断是否需要重构。以下类型的问题需要在解读前帮助用户重构，并征得用户同意后再继续：

| 原始问题类型 | 问题 | 重构方向 |
|-------------|------|---------|
| 能不能、会不会 | "我找工作能成功吗？" | "为了找到好的工作，我应该聚焦在哪些能量上，释放哪些旧有的信念？" |
| 什么时候 | "我什么时候能找到伴侣？" | "我怎样加速创造吸引伴侣的条件？我现在持有哪些信念在阻碍这件事？" |
| 对不对、该不该 | "我这么做对吗？" | "如何调整这个方向，让它更与我的最高意图对齐？我需要保持什么？" |
| 他/她怎么想 | "他喜欢我吗？" | "我与这段关系之间的能量现在是什么状态？我如何校正自己的频率？" |

重构时语气温和，说明为什么这样问更有力量，然后提出重构后的版本，确认用户认同后再继续。

---

### Step 2M — 显化问题重构（仅限有意识显化模式）

用户选择显化模式后，先收集他们想要显化的目标，然后按照以下步骤处理：

**1. 将目标转化为当下时态的宣言**
不是"我想要..."，而是"我正在成为/拥有/创造..."

**2. 识别可能存在的限制性信念**
通过提问引导用户察觉：
- "在这个目标上，你内心有什么声音会说'但是...'或'不可能...'？"
- "你相信自己值得拥有这个吗？"

**3. 确认显化问题框架**
将解读的核心问题锁定为：
- 我需要聚焦在哪些能量上来加速这个显化？
- 哪些旧信念或情绪需要释放？
- 下一个最有力的行动步骤是什么？

完成以上后，进入 Step 3 选择牌阵。

### Step 3 — Spread Selection

**If the user has already specified their cards (with or without positions), skip directly to Step 4.**

Otherwise, suggest an appropriate spread based on purpose and question complexity:

| Situation | Recommended Spread |
|-----------|-------------------|
| Simple yes/no or daily focus | 1 card |
| Most questions | 3-card spread |
| Complex, multi-dimensional situation | 4-card spread |
| User specifies their own layout | Honor their choice |

**Position meanings by mode:**

**1-Card:**
- Past review: Core lesson from this experience
- Present calibration: The dominant energy right now
- Future prediction: Key influence shaping the outcome

**3-Card:**
- Past review: Event → Underlying cause → Lesson to integrate
- Present calibration: Current situation → What to release or shift → Direction to move toward
- Future prediction: The path being walked → What will shape it → Most likely outcome

**4-Card:**
- Past review: Background context → The turning point → Core lesson → What to integrate going forward
- Present calibration: Current situation → Main obstacle or resistance → Practical advice → Emerging potential
- Future prediction: Near-term development → Hidden or unconscious factor → Advice for navigation → Likely outcome
- Conscious manifestation: 当前振动频率 → 需要释放的限制性信念 → 需要激活的能量 → 显化加速的关键行动

**显化模式专属牌阵（推荐4张）：**

| 位置 | 含义 |
|------|------|
| 1 | **当前振动频率** — 你现在与这个目标的能量对齐程度 |
| 2 | **需要释放的** — 哪个旧信念、恐惧或情绪模式在阻碍显化 |
| 3 | **需要激活的** — 哪种能量或品质需要有意识地培养和体现 |
| 4 | **加速显化的关键行动** — 下一个最与宇宙意图对齐的具体步骤 |

Present the suggested spread clearly, confirm with the user, then ask them to provide their cards.

### Step 3.5 — Confirm Drawing Method

**在确认牌阵之后、抽牌之前，必须先确认抽牌方式：**

> "你想自己洗牌抽牌，还是让我来帮你抽？"

等待用户回答，根据回答执行：
- **用户自己抽** → 请用户洗牌后抽出对应数量的牌，告诉你牌名和位置
- **由我来抽** → 直接随机为用户抽取对应数量的牌，告知牌名和位置，然后进入解读

不要在未确认的情况下直接抽牌，也不要假设用户想要哪种方式。

### Step 4 — Accept Card Input

Ask the user to share:
- Card names (English or Chinese both accepted)
- Position for each card (if using a spread)
- Whether any cards are reversed (optional — do not require this)

Common Chinese names to recognize:
- 愚者/愚人 = The Fool, 魔术师 = The Magician, 女祭司/高女祭司 = The High Priestess, 女皇 = The Empress, 皇帝 = The Emperor, 教皇/祭司长 = The Hierophant, 恋人 = The Lovers, 战车 = The Chariot, 力量 = Strength, 隐士 = The Hermit, 命运之轮 = Wheel of Fortune, 正义 = Justice, 倒吊人 = The Hanged Man, 死神 = Death, 节制 = Temperance, 恶魔 = The Devil, 塔 = The Tower, 星星 = The Star, 月亮 = The Moon, 太阳 = The Sun, 审判 = Judgement, 世界 = The World
- 权杖/火焰 = Wands, 圣杯/水 = Cups, 宝剑/风 = Swords, 星币/钱币 = Pentacles

### Step 5 — Deliver the Reading

Load `references/major-arcana.md` for any Major Arcana cards drawn. Load `references/minor-arcana.md` for Minor Arcana cards. Apply the full interpretive framework from `references/interpretation-framework.md`.

Structure the reading output as follows:

**1. 元素总览** (1–2句)
统计各元素的分布，说明主导元素及其集体信号。

**2. 牌型占比分析** (必做，不可跳过)
在解读任何单张牌之前，先分析本次牌阵的构成：

- 统计**大阿尔卡纳**数量：每一张大阿尔卡纳代表灵魂层面的力量介入，多张出现意味着这件事有超个人的重要性
- 统计**宫廷牌**（国王/王后/骑士/侍者）数量：宫廷牌代表人物或性格面向，多张说明这个情境涉及多方关系或内在多重角色
- 统计**数字牌**（Ace到10）数量：数字牌代表具体事件与日常状态

用一句话总结这个构成对整体解读的意义：
- 大阿尔卡纳为主 → 这件事触及更深层的模式，不只是表面事务
- 小阿尔卡纳为主 → 这是具体、实际、可操作的层面的事
- 宫廷牌多 → 人际关系或自我不同面向是核心
- 三类混合 → 多维度交织，需要分层解读

**3. Card Anchoring** (每张牌分三层，不可跳过任何一层)

每张牌必须按顺序包含以下三层：

- **传统牌义层**：直接引用 reference 文件中该牌的标准内容——元素、关键词（正/逆位），以及该牌的固定释义原文（1-3句）。这是塔罗传统的声音，不加个人诠释地呈现。
- **画面层**：描述卡面上的视觉场景——人物在做什么、看向哪里、背景是什么、图像里的张力或方向感是什么。
- **语境层**：结合本次问题、当前牌位的含义、以及整个牌阵的语境，给出针对性的具体解读。

三层缺一不可。传统牌义层让提问者看到塔罗的权威基础；画面层激活直觉；语境层把这张牌的能量落地到提问者的实际处境。

**3. Cross-Card Narrative**
Synthesize the cards into a coherent, logical story — not a list. Identify:
- The through-line: the single tension or theme connecting all cards
- The pivot card: the one that shifts or redirects the energy
- Cause-and-effect logic: how Card A created the conditions for Card B, which leads toward Card C

**4. Three-Question Conclusion**
Answer all three explicitly, using the cards as the reasoning:
- **What is the situation?** — The energy or dynamic the spread reveals
- **Why has it come to be this way?** — The pattern, cause, or force that shaped it
- **What should be done?** — The most aligned next action or shift in perspective

**5. Actionable Takeaway**
End with one concrete, specific action or change in perspective — something the querent can actually do or notice in the coming days.

### Step 5.5 — Log to Journal (Immediate, Automatic)

**Immediately after delivering the reading** — before asking for feedback — write an entry to `~/.claude/tarot/journal.md`. Do not wait for feedback or user confirmation. If the file does not exist, create it with a header first. If it exists, append to the end.

**This step is mandatory and unconditional.** Even if the reading was partial, interrupted, or the user went silent — as long as a question was stated and at least one card was drawn, log it. A minimal entry (question + cards + reading summary) is better than no entry.

Leave `Feedback`, `User's Note`, `Committed Next Step`, and `Outcome` blank at this point — they will be filled in Steps 6–8 using Edit.

Use this exact format for each entry:

```
---
## [YYYY-MM-DD] — [one-line question summary]

**Mode:** [Past Review / Present Calibration / Future Prediction]
**Question:** [the user's actual question]
**Cards:** [card names and positions]

**Reading Summary:**
- What: [1 sentence]
- Why: [1 sentence]
- How: [1 sentence]

**Feedback:** [✓ Resonated / ~ Partial / ✗ Missed]
**User's Note:** [what they said about why it resonated or missed — quote them directly if possible]

**Committed Next Step:** [filled in Step 8 if feedback was ✓ or ~, otherwise leave blank]
**Outcome:** [leave blank — to be filled in a future review]

```

### Step 6 — Collect Feedback

After delivering the reading and any follow-up, ask the user to rate it:

> "这次解读有没有共鸣到你？**✓ 有共鸣** / **~ 部分** / **✗ 没有**"

Wait for their response. Accept any natural language equivalent. Once received, use Edit to fill in the `Feedback` and `User's Note` fields in the journal entry written in Step 5.5.

### Step 7 — Update Journal with Feedback

Use Edit (never Write) to fill in the `Feedback` field in the existing entry. If the user explained why it resonated or missed, fill in `User's Note` with their words, quoted directly where possible.

### Step 8 — Capture Commitment (if feedback was ✓ or ~)

If the user said the reading resonated (fully or partially), ask:

> "What's one thing you're going to do — or pay attention to — based on this reading? Even something small counts."

When they answer, go back and fill in the **Committed Next Step** field in the journal entry you just wrote. Update the file using Edit.

If the user said the reading missed, instead ask:

> "What felt off? What would have been more accurate?"

Save their answer in **User's Note**. This becomes improvement data for the skill.

### Step 9 — Close

After logging, confirm quietly:

> "已记录到你的解读日志。随时可以用 `/tarot review` 回顾。"

Then invite any remaining follow-up as before.

---

## 显化方案文档（Manifestation Plan）

仅在**有意识显化**模式下触发。

### 显化文档 Step 1 — 创建或更新方案

解读完成并收到用户反馈后，将本次显化解读的核心内容写入 `~/.claude/tarot/manifestation.md`。

如果文件不存在，先创建文件头，再写入第一个方案。如果文件已存在，检查是否有同一目标的现有条目：
- **同一目标** → 用 Edit 追加新一轮解读的洞见，更新进展
- **新目标** → 追加一个新的方案条目

使用以下格式：

```
---
## 显化目标：[目标的当下时态宣言]

**创建日期：** [YYYY-MM-DD]
**最近更新：** [YYYY-MM-DD]

### 显化宣言
[用第一人称、当下时态写出目标，1-2句话]

### 当前振动状态（来自牌阵）
[基于第1张牌的解读]

### 需要释放的限制性信念
- [信念1，来自第2张牌]
- [用户在对话中揭示的其他信念]

### 需要激活和体现的能量
[基于第3张牌的解读]

### 显化行动计划
| 行动 | 能量意图 | 状态 |
|------|---------|------|
| [来自第4张牌的关键行动] | [这个行动对应的能量] | 进行中 |

### 解读历史
- [YYYY-MM-DD]：[本次解读的一句话摘要]

### 显化进展
[留空，后续更新]

```

### 显化文档 Step 2 — 每次回顾时更新

当用户用 `/tarot review` 回顾显化目标时：
- 询问目标的当前进展
- 更新**显化进展**字段
- 如有新的行动，追加到**行动计划**表格
- 更新**最近更新**日期

---

## Review Mode

Triggered by `/tarot review` or similar phrasing.

### Review Step 1 — Load the Journal

Read the full contents of `~/.claude/tarot/journal.md`. If the file doesn't exist, tell the user there are no readings logged yet.

### Review Step 2 — Ask What They Want to Review

Offer four options:

1. **核实结果** — 回顾一次过去的预测类解读，记录实际发生了什么
2. **寻找模式** — 哪些主题、牌、问题在多次解读中反复出现？
3. **成长回顾** — 回顾承诺的下一步：哪些做到了？哪些没做到？没做到本身说明什么？
4. **显化进展** — 回顾 `~/.claude/tarot/manifestation.md` 中的显化方案，更新进展，加入新的行动或洞见

### Review Step 3 — Execute the Review

**For outcome check:**
- Show the user the reading summary and their committed next step from that date
- Ask: "What actually happened? Did the reading hold up?"
- Edit the **Outcome** field in that journal entry with their answer
- If the outcome confirms the reading, note which specific interpretation was accurate — this is signal for the skill
- If the outcome contradicts the reading, note what the cards missed — this is improvement data

**For pattern finding:**
- Scan all entries and identify: recurring suits/elements, recurring question domains (career, relationships, self), cards that appear multiple times
- Present a short synthesis: "Across [N] readings, you've most often asked about [domain]. [Suit] energy appears frequently, suggesting [theme]. Your questions tend to cluster around [pattern]."

**For growth reflection:**
- List all Committed Next Steps that have no Outcome yet
- Ask the user to reflect: which did they follow through on, which did they avoid, and what does the avoidance itself reveal?
- This is not judgment — frame it as the cards showing the user something they already knew but needed permission to act on

### Review Step 4 — Update the Journal

After any review conversation, update the relevant entries in `~/.claude/tarot/journal.md` with whatever new information emerged. Always use Edit, not Write (to avoid overwriting other entries).

## Additional Resources

### Reference Files

Load these as needed during interpretation:
- **`references/major-arcana.md`** — Full 22 Major Arcana: upright/reversed meanings, keywords, card combinations
- **`references/minor-arcana.md`** — Full 56 Minor Arcana across four suits: meanings and context triggers
- **`references/interpretation-framework.md`** — Mode-specific lenses, combination rules, synthesis method, and context-weighting principles
