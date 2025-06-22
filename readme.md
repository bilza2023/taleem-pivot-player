
# Taleem Pivot Player

A timing-aware slide renderer built in Svelte — designed to play animated educational decks exported by the Taleem DeckBuilder.

---

## 📦 Installation

```bash
npm i taleem-pivot-player
````

To create decks that work with this player:

```bash
npm i taleem-pivot-deckbuilder
```

> ⚠️ The player and the deckbuilder must be used together. Slide data contracts and timing rules are tightly integrated.

---

## 🚀 What It Does

This player:

* Renders slide decks composed using the `DeckBuilder` API
* Animates slide items based on `showAt` timings
* Syncs all content to background audio (via Howler.js)
* Automatically switches slides as time progresses

---

## 🧱 Slide Types Supported

Each slide uses:

```js
deckbuilder.s.slideType(endTime, [ { name, content, showAt } ])
```

The player reads the deck and renders slides one by one based on:

* `start` and `end` define the visibility window for a slide
* Each item appears when `currentTime >= slide.start + showAt`

---

### ✅ Timing-Sensitive Slides

These slides reveal items based on `showAt`:

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

---

### 📌 Static Slides (No `showAt` logic required)

These render all content at once:

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
* `quoteSlide` (can support optional line timing)

---

## 🧩 Deck Format

Although the player receives a **deck as JSON**, you should always generate this JSON using the `taleem-pivot-deckbuilder` package.

The DeckBuilder ensures:

* Slides are correctly timed (`start`, `end`)
* Items use `showAt` relative to their slide
* The output structure is valid and player-compatible

Example deck:

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

* Each slide has an `end` time (required)
* Items inside the slide use `showAt` (default: 0)
* Items appear at `slide.start + showAt`
* If `showAt` exceeds the slide duration, item won’t appear

Use the DeckBuilder to manage these offsets automatically.

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

## 📣 License

ISC License — MIT-compatible
Built by Taleem.Help

