<script>
  export let data;
  export let currentTime;

  $: imageItem = data.find(d => d.name === "image" && d.showAt <= currentTime);

  $: bullets = data
    .filter(d => d.name === "bullet" && d.showAt <= currentTime)
    .sort((a, b) => a.showAt - b.showAt)
    .map(d => d.content);
</script>

<div class="slide-container">
  <div class="bullets-left">
    <ul>
      {#each bullets as bullet}
        <li>{bullet}</li>
      {/each}
    </ul>
  </div>

  {#if imageItem}
    <div class="image-right">
      <img src={imageItem.content} alt="Slide image" />
    </div>
  {/if}
</div>

<style>
.slide-container {
  display: flex;
  height: 100%;
  width: 100%;
  box-sizing: border-box;
  padding: 40px;
  gap: 60px;
}

.image-right img {
  max-height: 80vh;
  max-width: 40vw;
  object-fit: contain;
  border-radius: 12px;
}

.bullets-left {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}

.bullets-left ul {
  list-style-type: disc;
  padding-left: 1.5rem;
  font-size: 4.5rem;
}

.bullets-left li {
  margin-bottom: 1rem;
}

@media (max-width: 768px) {
  .slide-container {
    flex-direction: column;
    align-items: center;
  }

  .bullets-left ul {
    font-size: 2rem;
  }

  .image-right img {
    max-width: 80vw;
  }
}
</style>