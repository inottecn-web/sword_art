# Sword Art RPG — дизайн-концепт «Sunrise Guild Atlas»

Документ фиксирует конкретное UI/UX-направление для макетов DesignerGPT/Figma на основе `sword_art_rpg_designergpt_uiux_brief.md` и подхода установленного `frontend-design` skill: смелая, запоминающаяся, production-grade эстетика без generic AI UI.

## 1. UX summary

**Концепция:** modern fantasy dashboard в виде живого атласа гильдии. Игрок не переписывается с ИИ, а управляет персонажем через системно разрешённые action cards. Серверные результаты, d6 и риски имеют строгий интерфейсный слой; AI-нарратив — отдельный атмосферный слой.

**Что должен запомнить игрок:** «Я стою у Рассветного Порога, вижу карту этажа, живых игроков рядом, безопасные и спорные маршруты, а мои действия выглядят как реальные игровые команды, не как чат».

## 2. User flows

### 2.1. Первый вход

```text
Landing → Создать персонажа → Выбор класса → Рассветный Порог → Безопасное действие → Результат → Следующее действие
```

Контрольные UX-точки:

- жанр объясняется за 10–15 секунд;
- CTA ведёт к созданию персонажа;
- после выбора класса игрок сразу видит безопасное действие;
- на первом игровом экране нет поля свободного ввода.

### 2.2. Первый сбор рассветника

```text
Сцена → Карта этажа → Светлая Опушка → ActionCard: Собрать рассветник → Confirm → DiceResultPanel → SystemResultBlock → NarrativeBlock → Craft hint
```

### 2.3. Вход в спорную зону

```text
Карта → Тропа Рассветника → PvpWarningModal → Сравнение безопасного/спорного маршрута → Подтвердить вход или отступить
```

### 2.4. Первый бой

```text
Encounter → Initiative strip → Выбор боевого ActionCard → DiceResultPanel → SystemResultBlock → NarrativeBlock → NextActions
```

## 3. Информационная архитектура

Основные разделы:

1. **Сцена** — текущая локация, арт, нарратив, системный результат, действия.
2. **Карта** — зоны, маршруты, ресурсы, PvP-риск, события.
3. **Персонаж** — класс, характеристики, ветки, экипировка, статусы.
4. **Инвентарь** — ресурсы, предметы, durability, сравнение.
5. **Крафт** — рецепты, материалы, стоимость, шанс/сложность.
6. **Рынок** — ордера, цены, торговые маршруты.
7. **Социальное** — группа, рейд, гильдия, presence.
8. **Хроника** — личная и серверная история.
9. **Цели** — личные, групповые, серверные.
10. **Настройки** — доступность, звук, уведомления, плотность.

## 4. Desktop layout

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ Top Bar: logo · Floor 1 · Dawn Gate · server state · currency · profile     │
├───────────────┬─────────────────────────────────────────────┬───────────────┤
│ Atlas & Party │ Scene Stage                                 │ Action Deck   │
│               │ - scene art / illustrated map slice          │               │
│ mini-map      │ - zone title + risk badge                    │ 3–6 actions   │
│ objectives    │ - NarrativeBlock                            │ risk/req/cost │
│ presence      │ - SystemResultBlock                         │ disabled why  │
│ chronicle     │ - DiceResultPanel when relevant             │ confirmation  │
│ party status  │ - NextActions summary                       │ hints         │
├───────────────┴─────────────────────────────────────────────┴───────────────┤
│ Guild Belt: Character · Inventory · Craft · Market · Chronicle · Settings   │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 4.1. Top Bar

- Высота 64–72 px.
- Слева: знак башни + название игры.
- Центр: `Floor 1 / Рассветный Порог / Safe`.
- Справа: серверный прогресс, валюта, уведомления, профиль.

### 4.2. Atlas & Party

- Мини-карта с тремя состояниями зон: safe, contested, dangerous.
- Блок ближайших целей.
- Presence: «12 игроков рядом», «2 ищут группу».
- Хроника: последние 3 серверных события.

### 4.3. Scene Stage

- Большой арт/карта-зона занимает верхнюю часть центра.
- NarrativeBlock отделён от системных результатов визуально и текстово.
- SystemResultBlock всегда короче и жёстче: цифры, изменения, статусы.
- DiceResultPanel появляется как расчётная панель, а не как декоративная анимация.

### 4.4. Action Deck

ActionCard должна быть крупнее обычной кнопки и содержать:

- название действия;
- тип: Explore / Combat / Gather / Social / Craft / Travel;
- risk badge;
- требования;
- стоимость;
- preview результата;
- disabled reason, если действие недоступно;
- confirm CTA.

## 5. Mobile/PWA layout

```text
┌────────────────────┐
│ Compact Top Bar    │
├────────────────────┤
│ Scene Art          │
├────────────────────┤
│ Zone + Risk        │
├────────────────────┤
│ Narrative collapsed│
├────────────────────┤
│ System Result      │
├────────────────────┤
│ Primary Action     │
├────────────────────┤
│ Bottom Tabs        │
└────────────────────┘
```

Правила:

- bottom tabs: Сцена, Действия, Карта, Персонаж, Социальное;
- Action Deck открывается отдельным tab или bottom sheet;
- PvP warning занимает отдельный full-height confirmation sheet;
- touch targets минимум 44 px;
- длинный нарратив по умолчанию свернут после 4–5 строк.

## 6. Visual direction

### 6.1. Название направления

**Sunrise Guild Atlas** — светлая fantasy-MMO эстетика первого этажа: рассвет, зелёные поля, карта экспедиции, гильдейский журнал и прозрачный системный HUD.

### 6.2. Палитра

| Role | Hex | Usage |
|---|---:|---|
| Deep Forest | `#123B2A` | primary UI, navigation, main text on light surfaces |
| Verdigris | `#2D7A68` | active states, safe route outlines |
| Dawn Gold | `#F2B84B` | primary CTA, progress, key highlights |
| Meadow Safe | `#7CBF6A` | safe badges, safe map nodes |
| Amber Contested | `#D9822B` | contested/PvP warning |
| Oath Red | `#8F2D2D` | danger, critical loss, defeat |
| Arcane Lilac | `#A78BFA` | AI narration only, never rewards |
| Parchment | `#F3E7C9` | light cards and narrative surfaces |
| Night Slate | `#17212B` | dark mode background |
| Ink | `#20302A` | body text |

### 6.3. Typography

- Display/headings: characterful fantasy-serif with clear readability, e.g. **Cinzel**, **Cormorant Garamond**, or similar.
- UI/body: readable humanist serif/sans, e.g. **Source Serif 4**, **Alegreya Sans**, **Atkinson Hyperlegible**.
- Numeric/system data: tabular numerals for dice, HP, costs, market prices.

Avoid: Arial/Roboto/Inter-only look, because it makes the fantasy UI feel generic.

### 6.4. Texture and depth

- Fine parchment grain on cards.
- Soft glass/parchment panels over illustrated backgrounds.
- Thin cartographic lines on map surfaces.
- Gold hairline dividers for progression.
- Amber double-outline for contested interactions.

## 7. Key screens

### 7.1. Landing / вход

Layout:

- hero with tower silhouette over sunrise fields;
- title: «Покори башню вместе с сервером»;
- subtitle: «Текстово-графическая MMO-RPG без свободного ввода: выбирай действия, а сервер считает последствия»;
- CTA: «Создать персонажа», secondary «Продолжить»;
- server chronicle card: opened floor, boss preparation, active guilds.

### 7.2. Character creation

- Class cards in 2–3 column responsive grid.
- Each card: illustration, role, difficulty, party value, non-combat value, future branches.
- Selected class expands into detail panel.
- CTA disabled until player confirms name/class.

### 7.3. Main game screen

- Left: atlas, objectives, presence.
- Center: scene art, narrative, system result.
- Right: action cards.
- Bottom: Guild Belt navigation.

Default first action examples:

- «Осмотреть Рассветный Порог» — safe, no cost.
- «Поговорить у доски гильдии» — social, safe.
- «Собрать рассветник у Светлой Опушки» — gather, safe.
- «Пойти на Тропу Рассветника» — contested, warning required.

### 7.4. Map of Floor 1

Map nodes:

- Рассветный Порог — safe hub.
- Дорога Пастухов — safe route.
- Светлая Опушка — safe gather.
- Тропа Рассветника — contested gather.
- Заросший Овраг — dangerous/miniboss.

Filters:

- resources;
- PvP risk;
- quests;
- group;
- trade routes;
- threats;
- server events.

### 7.5. PvE battle

- Enemy and party strips.
- Initiative/turn order.
- Action Deck narrowed to combat actions.
- DiceResultPanel shows roll, modifiers, difficulty, margin.
- Combat log is compact and system-first.
- NarrativeBlock appears after system result.

### 7.6. Action result

Order of blocks:

1. Chosen action.
2. DiceResultPanel if applicable.
3. SystemResultBlock.
4. NarrativeBlock.
5. NextActions.

### 7.7. Inventory and craft

- Inventory grid with filters.
- Resource chips for рассветник and materials.
- Recipe card for simple healing potion.
- Cost preview before crafting.
- Missing materials highlighted with exact quantities.

### 7.8. Contested route warning

PvpWarningModal content:

- «Спорная зона: Тропа Рассветника»;
- what can be lost;
- what cannot be lost;
- newbie protection;
- possible gain;
- active players nearby;
- safe alternative CTA.

### 7.9. Group / raid

- Party members by role.
- Readiness state.
- Current objective.
- Shared action log.
- Vote/timer if group decision is required.

### 7.10. Chronicle

- Server timeline.
- Boss preparation progress.
- Guild contributions.
- First kills.
- Player personal milestones.

## 8. Component inventory

- `ActionCard`
- `RiskBadge`
- `DiceResultPanel`
- `NarrativeBlock`
- `SystemResultBlock`
- `ResourceChip`
- `ClassCard`
- `ZoneCard`
- `QuestObjective`
- `PartyPanel`
- `PresenceIndicator`
- `ChronicleEntry`
- `InventoryItemCard`
- `CraftRecipeCard`
- `TradeOrderRow`
- `BossProgressMeter`
- `ReputationStatusBadge`
- `PvpWarningModal`
- `ConfirmActionDialog`
- `GuildBeltNav`
- `AtlasMiniMap`

## 9. Accessibility

- Risk uses color + icon + text.
- System numbers use high contrast and tabular numerals.
- Reduced motion disables dice animation and parallax.
- Keyboard navigation supports action selection and confirmation.
- Mobile controls use large touch targets.
- Empty, loading and reconnect states are explicit.

## 10. Microcopy

- «Выберите действие: сервер покажет риск и последствия до подтверждения.»
- «ИИ описывает сцену, но не решает результат.»
- «Спорная зона: часть ресурсов может быть потеряна при нападении.»
- «Недоступно: нужен серп или навык сбора трав.»
- «Бросок d6: 18 против сложности 15 — успех.»
- «Безопасный маршрут даст меньше рассветника, но исключает PvP-потери.»

## 11. Anti-patterns

- Не добавлять поле «напиши, что хочешь сделать».
- Не смешивать нарратив и награды в одном блоке.
- Не скрывать PvP-риск за tooltip.
- Не делать landing единственным красивым экраном.
- Не копировать Sword Art Online визуально.
- Не превращать основной экран в чат с лентой сообщений.
- Не использовать generic purple AI gradient как основу бренда.
