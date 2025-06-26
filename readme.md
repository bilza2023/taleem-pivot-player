
# Taleem Pivot Player

A timing-aware slide renderer built in Svelte — designed to play animated educational decks exported by the Taleem DeckBuilder.

---

## 📦 Installation

```bash
npm i taleem-pivot-player
````

To create decks compatible with this player:

```bash
npm i taleem-pivot-deckbuilder
```

> ⚠️ The player and deckbuilder must be used together. They share strict timing and data contracts.

---

## 🚀 What It Does

This player:

* Renders decks built with the `DeckBuilder` API
* Animates items using `showAt` delays
* Plays audio in sync with slides (via Howler.js)
* Auto-advances slides using `start` and `end` values

---

## 🧱 Slide Types Supported

Each slide uses:

```js
deckbuilder.s.slideType(end, [ { name, content, showAt } ])
```

Slides render based on:

* `start` and `end` → control visibility window
* `showAt` → item appears when `currentTime >= start + showAt`

---

### ✅ Timing-Aware Slides

These slides support precise animation per item:

#### `bulletList`

```js
deckbuilder.s.bulletList(10, [
  { name: "bullet", content: "Physics", showAt: 0 },
  { name: "bullet", content: "Chemistry", showAt: 1 }
]);
```

#### `imageLeftBulletsRight`

```js
deckbuilder.s.imageLeftBulletsRight(10, [
  { name: "image", content: "/pivot/box.webp", showAt: 0 },
  { name: "bullet", content: "Energy is conserved", showAt: 1 }
]);
```

#### `barChart`

```js
deckbuilder.s.barChart(10, [
  { name: "bar", label: "Math", value: 80, showAt: 0 },
  { name: "bar", label: "Physics", value: 70, showAt: 2 }
]);
```

#### `cornerWordsSlide`

```js
deckbuilder.s.cornerWordsSlide(10, [
  { name: "card", icon: "🔭", label: "Observe", showAt: 0 },
  { name: "card", icon: "⚗️", label: "Experiment", showAt: 2 }
]);
```

#### `titleAndSubtitle`

```js
deckbuilder.s.titleAndSubtitle(10, [
  { name: "title", content: "Chapter 1", showAt: 0 },
  { name: "subtitle", content: "Scope of Physics", showAt: 2 }
]);
```

#### `twoColumnText`

```js
deckbuilder.s.twoColumnText(10, [
  { name: "title", content: "Branches", showAt: 0 },
  { name: "left", content: "Mechanics\nOptics", showAt: 1 },
  { name: "right", content: "Thermodynamics\nQuantum", showAt: 2 }
]);
```

#### `eq` *(NEW!)*

```js
const eq = deckbuilder.s.eq(60);

eq.addLine({ type: "heading", content: "Math Showcase", showAt: 0 });
eq.addSp({ type: "heading", content: "Famous Formulas" });

eq.addLine({ type: "math", content: "E = mc^2", showAt: 5 });
eq.addSp({ type: "math", content: "E = mc^2" });

eq.addLine({ type: "text", content: "Energy and mass are interchangeable", showAt: 15 });
eq.addSp({ type: "text", content: "Einstein's insight" });
```

* EQ slides allow dual-channel control:

  * `addLine()` → controls timing
  * `addSp()` → defines sidebar content
* For more: [EQ Slide Format](./docs/eq.md)

---

### 📌 Static Slides (All items appear at once)

These slides don’t animate individual items:

* `titleSlide`
* `imageSlide`
* `imageWithCaption`
* `imageWithTitle`
* `table`
* `statistic`
* `donutChart`
* `bigNumber`
* `quoteWithImage`
* `contactSlide`
* `quoteSlide` *(can optionally animate line-by-line)*

---

## 🧩 Deck Format

Decks are exported via `deckbuilder.build()` and passed to the player as JSON.

The DeckBuilder ensures:

* Valid slide structure
* Auto-calculated `start`
* Required `end` and `showAt` values
* Player-compatible formatting

```js
import { DeckBuilder } from "taleem-pivot-deckbuilder";

const deckbuilder = new DeckBuilder();
deckbuilder.s.bulletList(10, [
  { name: "bullet", content: "Physics", showAt: 0 },
  { name: "bullet", content: "Chemistry", showAt: 1 }
]);

export const deck = deckbuilder.build();
```

---

## ⏱ Timing Control

* `start` is inferred by the DeckBuilder
* `end` is required for every slide
* `showAt` is relative to slide start
* If `showAt + start > end`, item won’t appear

---

## 🕹 Usage in Svelte

```svelte
<script>
  import { deck } from './deck.js';
  import PivotPlayer from 'taleem-pivot-player';
</script>

<PivotPlayer
  {deck}
  soundUrl="/sounds/lecture.opus"
  background={{
    backgroundColor: '#000',
    backgroundImage: '/pivot/defaultBg.png',
    backgroundImageOpacity: 0.9
  }}
/>
```

---

## 📚 Developer Docs

* [DeckBuilder API](./docs/api.md)
* [Slide Timing System](./docs/timing.md)
* [EQ Slide Format](./docs/eq.md)

---

## 📣 License

ISC License — MIT-compatible
Built by Taleem.Help

---

### 🔗 References

* [https://www.npmjs.com/package/taleem-pivot-deckbuilder](https://www.npmjs.com/package/taleem-pivot-deckbuilder)
* `npm i taleem-pivot-deckbuilder`

```
```
