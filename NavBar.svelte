<!-- NavBar.svelte -->
<script>
  export let currentTime = 0;
  export let duration = 100;
  export let onPlay = () => {};
  export let onPause = () => {};
  export let onStop = () => {};
  export let onBack = () => {};
  export let onSeek = (val) => {};

  let visible = true;
  let hideTimeout;

  function formatTime(seconds) {
    const m = Math.floor(seconds / 60)
      .toString()
      .padStart(2, "0");
    const s = Math.floor(seconds % 60)
      .toString()
      .padStart(2, "0");
    return `${m}:${s}`;
  }

  function showTemporarily() {
    visible = true;
    clearTimeout(hideTimeout);
    hideTimeout = setTimeout(() => (visible = false), 4000);
  }

  // Listen to any pointer movement
  const handleMove = () => showTemporarily();
</script>

<div
  class="nav-bar"
  class:visible
  on:pointermove={handleMove}
  on:touchstart={handleMove}
>
  <button on:click={onBack}>←</button>
  <button on:click={onPlay}> ▶️</button>
  <button on:click={onPause}>⏸️</button>
  <button on:click={onStop}>⏹️</button>

  <span class="timer">{formatTime(currentTime)} / {formatTime(duration)}</span>
  <input
    type="range"
    min="0"
    max={duration}
    step="0.1"
    bind:value={currentTime}
    on:input={(e) => onSeek(+e.target.value)}
  />
</div>

<style>
  .nav-bar {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    background: rgba(0, 0, 0, 0.6);
    color: white;
    display: flex;
    align-items: center;
    padding: 0.5rem 1rem;
    gap: 0.5rem;
    opacity: 0;
    transition: opacity 0.4s;
    z-index: 10;
  }
  .nav-bar.visible {
    opacity: 1;
  }
  .nav-bar button {
    border: none;
    border-radius: 6px;
    padding: 0.4rem 0.75rem;
    font-size: 0.9rem;
    background: #333;
    color: #fff;
    cursor: pointer;
  }

  .nav-bar button:hover {
    background: #555;
  }

  .nav-bar .timer {
    min-width: 90px;
    text-align: center;
  }
  .nav-bar input[type="range"] {
    flex: 1;
  }
</style>
