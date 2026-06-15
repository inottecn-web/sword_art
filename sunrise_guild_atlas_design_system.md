# Sword Art RPG — Sunrise Guild Atlas Design System & Element Library

Этот файл дополняет `sunrise_guild_atlas_ui_prototype.html` и фиксирует библиотеку компонентов для Figma/Next.js + Tailwind + shadcn/ui. Цель — покрыть 100% UI/UX-брифа: основной игровой цикл, все MVP-экраны, запрет свободного ввода, системные результаты, d6, PvP-риск, mobile/PWA и доступность.

## 1. Design principles

1. **Server-first interaction.** Игрок выбирает только разрешённые действия; UI никогда не показывает поле свободного ввода как gameplay-input.
2. **Narrative is atmospheric, not authoritative.** AI-текст живёт в `NarrativeBlock`; все правила, ресурсы, урон, риск и прогресс живут в системных компонентах.
3. **Risk is impossible to miss.** Safe/Contested/Danger всегда кодируются цветом, иконкой, текстом и подтверждением.
4. **MMO presence by default.** Даже одиночная сцена показывает presence, хронику, группы, рынок или вклад сервера.
5. **Progressive complexity.** Первый экран показывает 3–6 действий; дополнительные системы раскрываются через вкладки, sheets и фильтры.

## 2. Design tokens

| Token | Value | Usage |
|---|---:|---|
| `--forest` | `#123B2A` | primary UI, headers, active nav, system layer |
| `--verdigris` | `#2D7A68` | active route, safe outline, secondary action |
| `--gold` | `#F2B84B` | primary CTA, progress, recommended action |
| `--safe` | `#7CBF6A` | safe state, allowed low-risk action |
| `--contested` | `#D9822B` | PvP/contested state, warnings |
| `--danger` | `#8F2D2D` | dangerous state, defeat, critical loss |
| `--lilac` | `#A78BFA` | AI narrative only, never rewards |
| `--parchment` | `#F3E7C9` | cards, narrative notes |
| `--paper` | `#FFF8DF` | elevated panels and modal content |
| `--slate` | `#17212B` | dark calculation panels, combat log |
| `--ink` | `#20302A` | body text |

## 3. Typography

- **Display:** Georgia/Cinzel-like fantasy serif for hero, location and screen titles.
- **UI/body:** Alegreya Sans/Atkinson Hyperlegible/system sans for readable interaction text.
- **Numbers:** tabular numerals for d6, HP, currency, prices, quantities and cooldowns.
- **Minimum sizes:** body 16px desktop, 15px mobile; badges 12–13px; touch targets at least 44px.

## 4. Layout primitives

- `TopBar` — global floor, zone, server state, currency, profile.
- `AtlasAndPartyRail` — mini-map, objectives, party/presence, server events.
- `SceneStage` — scene art, risk state, Narrative/System/Dice stack.
- `ActionDeck` — 3–6 cards with state, risk, requirements, cost and CTA.
- `GuildBeltNav` — desktop bottom nav; becomes mobile bottom tabs.
- `ResponsiveSheet` — mobile action deck, PvP warning, confirmation and filters.

## 5. Component library

### 5.1. `ActionCard`

Required anatomy:

1. Title.
2. Action type: Explore / Combat / Gather / Social / Craft / Travel / Market / Party.
3. Risk badge.
4. Requirements.
5. Cost/time/cooldown.
6. Reward/result preview.
7. Difficulty if d6 check exists.
8. Disabled reason when unavailable.
9. Confirmation CTA.

States: `available`, `recommended`, `disabled`, `contested`, `dangerous`, `cooldown`, `requires_item`, `party_vote_required`, `selected`, `loading_result`.

### 5.2. `RiskBadge`

- Safe: `🛡 Safe`, green outline.
- Contested: `⚠ Contested`, amber outline + warning copy.
- Dangerous: `☠ Dangerous`, red outline + explicit loss copy.
- Never rely on color alone.

### 5.3. `DiceResultPanel`

Shows: dice pool, each d6, modifiers, total, DC, margin, outcome label, and a short explanation. Reduced motion replaces animation with static dice chips.

### 5.4. `SystemResultBlock`

Shows only facts: damage, resources gained/lost, reputation, durability, quest progress, zone state and next unlocks. It must be visually stricter than narrative.

### 5.5. `NarrativeBlock`

Soft parchment note with lilac accent. It may be longer than system text, but on mobile it collapses after 4–5 lines.

### 5.6. `PvpWarningModal`

Required content: contested zone name, what can be lost, what cannot be lost, newbie protection, likely gain, active players nearby, safe alternative, confirm/retreat actions.

### 5.7. `ClassCard`

Shows class illustration/icon, role, difficulty, playstyle, group value, non-combat value and future branches. CTA remains disabled until name and class are confirmed.

### 5.8. `ZoneCard`

Shows zone name, risk, resources, routes, active players, events and recommended party size.

### 5.9. Economy and craft components

- `ResourceChip` — icon, name, amount, source.
- `InventoryItemCard` — rarity, durability, bind/trade state, compare affordance.
- `CraftRecipeCard` — materials, exact missing quantities, result, difficulty/chance, confirm CTA.
- `TradeOrderRow` — buy/sell, unit price, stack, route risk, seller/guild.

### 5.10. Social and server components

- `PartyPanel` — role, HP, ready state, connection state, leader marker.
- `PresenceIndicator` — players nearby, LFG count, hostile/neutral hints for contested areas.
- `ChronicleEntry` — personal/server/guild event, timestamp, impact.
- `BossProgressMeter` — server preparation, contribution, next milestone.
- `ReputationStatusBadge` — faction, delta, current standing.

## 6. MVP screen library

1. `LandingScreen` — promise, CTA, server chronicle.
2. `CharacterCreationScreen` — class grid and selection detail.
3. `MainSceneScreen` — full desktop gameplay dashboard.
4. `FloorMapScreen` — nodes, routes, filters, risk overlays.
5. `PveBattleScreen` — participants, initiative, combat action deck, dice, combat log.
6. `ActionResultScreen` — chosen action, dice, system facts, narrative, next actions.
7. `InventoryCraftScreen` — inventory, recipe, missing materials, durability and compare.
8. `ContestedRouteWarning` — full explicit PvP warning.
9. `GroupRaidScreen` — roles, readiness, vote/timer, shared log.
10. `ChronicleScreen` — server timeline, boss prep, guild contributions, personal milestones.

## 7. Responsive rules

- Desktop: three-column command table; primary session mode.
- Tablet: stack Atlas above/inside Scene; Action Deck can become right drawer.
- Mobile/PWA: compact top bar, scene first, collapsed narrative, system result, primary action, bottom tabs. Action Deck and PvP warning open as full-height sheets.

## 8. Accessibility checklist

- Text contrast must remain readable on parchment and dark panels.
- Risk must use icon + text + color.
- Keyboard can tab through top nav, action cards, confirmation and sheets.
- `prefers-reduced-motion` disables dice/parallax.
- All mobile controls are at least 44px.
- Loading, empty and reconnect states use explicit copy.

## 9. Handoff notes

- The prototype is static, but components are named to map directly to React components.
- Use Tailwind tokens matching the CSS variables in the prototype.
- Keep shadcn/ui primitives for dialogs, tabs and sheets, but override visual styling with the Sunrise Guild Atlas tokens.
- Do not introduce AI chat input unless it is a non-gameplay support feature outside the core scene loop.
