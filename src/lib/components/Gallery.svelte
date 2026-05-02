<script lang="ts">
  import { onMount } from 'svelte';
  import { assets } from '$app/paths';

  const images = [
    { src: `${assets}/images/gallery/1.jpeg`, alt: 'Gallery image 1', caption: '' },
    { src: `${assets}/images/gallery/2.jpeg`, alt: 'Gallery image 2', caption: '' },
    { src: `${assets}/images/gallery/3.jpeg`, alt: 'Gallery image 3', caption: '' },
    { src: `${assets}/images/gallery/4.jpeg`, alt: 'Gallery image 4', caption: '' },
  ];

  /** @type {'full' | 'large' | 'fit'} */
  export let size = 'large';

  let activeIndex = 0;

  function prev() {
    activeIndex = (activeIndex - 1 + images.length) % images.length;
  }

  function next() {
    activeIndex = (activeIndex + 1) % images.length;
  }

  function goTo(i: number) {
    activeIndex = i;
  }

  function handleKeydown(e: KeyboardEvent) {
    if (e.key === 'ArrowLeft') prev();
    if (e.key === 'ArrowRight') next();
  }

  onMount(() => {
    window.addEventListener('keydown', handleKeydown);
    return () => window.removeEventListener('keydown', handleKeydown);
  });
</script>

<div class="carousel-wrapper" data-size={size}>
  <!-- Indicators -->
  <div class="carousel-indicators">
    {#each Array.from({ length: images.length }, (_, i) => i) as i (i)}
      <button
        class="indicator-btn"
        class:active={i === activeIndex}
        on:click={() => goTo(i)}
        aria-label="Go to slide {i + 1}"
      ></button>
    {/each}
  </div>

  <!-- Slide -->
  <div class="carousel-slide">
    {#key activeIndex}
      <img
        src={images[activeIndex].src}
        alt={images[activeIndex].alt}
        class="carousel-image"
      />
    {/key}
  </div>

  <!-- Caption beneath image -->
  {#if images[activeIndex].caption}
    <p class="carousel-caption">{images[activeIndex].caption}</p>
  {/if}

  <!-- Counter -->
  <p class="carousel-counter">{activeIndex + 1} / {images.length}</p>

  <!-- Controls -->
  <button class="carousel-control carousel-prev" on:click={prev} aria-label="Previous">
    <span aria-hidden="true">&#x2039;</span>
  </button>
  <button class="carousel-control carousel-next" on:click={next} aria-label="Next">
    <span aria-hidden="true">&#x203A;</span>
  </button>
</div>

<style>
  .carousel-wrapper {
    position: relative;
    width: 100%;
    margin: 2rem auto;
    user-select: none;
  }

  .carousel-wrapper[data-size='fit']   { max-width: 680px; }
  .carousel-wrapper[data-size='large'] { max-width: 1100px; }
  .carousel-wrapper[data-size='full']  { max-width: 100%; }

  /* Indicators */
  .carousel-indicators {
    display: flex;
    justify-content: center;
    gap: 8px;
    margin: 0 0 12px;
    padding: 0;
  }

  .indicator-btn {
    width: 10px;
    height: 10px;
    border-radius: 50%;
    border: none;
    background: #ccc;
    cursor: pointer;
    padding: 0;
    transition: background 0.2s, transform 0.2s;
  }

  .indicator-btn.active {
    background: #333;
    transform: scale(1.25);
  }

  /* Slide */
  .carousel-slide {
    position: relative;
    width: 100%;
    aspect-ratio: 16 / 9;
    overflow: hidden;
    border-radius: 8px;
    background: #111;
  }

  .carousel-image {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
    animation: fadeSlide 0.35s ease;
  }

  @keyframes fadeSlide {
    from { opacity: 0; transform: translateX(20px); }
    to   { opacity: 1; transform: translateX(0); }
  }

  /* Caption beneath image */
  .carousel-caption {
    text-align: center;
    margin: 0.6rem 0 0;
    font-size: 0.9rem;
    color: #555;
  }

  .carousel-counter {
    text-align: center;
    margin: 0.25rem 0 0;
    font-size: 0.75rem;
    color: #aaa;
  }

  /* Prev / Next buttons */
  .carousel-control {
    position: absolute;
    top: calc(50% - 1rem);
    transform: translateY(-50%);
    background: rgba(0, 0, 0, 0.4);
    border: none;
    color: white;
    font-size: 2.25rem;
    line-height: 1;
    padding: 0.2rem 0.7rem;
    cursor: pointer;
    border-radius: 4px;
    transition: background 0.2s;
  }

  .carousel-control:hover { background: rgba(0, 0, 0, 0.7); }
  .carousel-prev { left: 0.75rem; }
  .carousel-next { right: 0.75rem; }
</style>