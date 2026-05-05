<script lang="ts">
  import { onMount, createEventDispatcher } from 'svelte';
  import { assets } from '$app/paths';
  export let videoSrc = `${assets}/headervideo.mp4`;
  export let headline = 'POWER SLAP';
  export let storageKey = 'intro-video-played';
  export let fadeDuration = 1500;

  const dispatch = createEventDispatcher();

  let videoEl: HTMLVideoElement;
  let wrapperEl: HTMLDivElement;
  let showHeadline = false;
  let shouldRenderVideo = false;
  let needsTap = false; // shown when autoplay is blocked

  onMount(() => {
    const hasPlayed = localStorage.getItem(storageKey);
    shouldRenderVideo = true;
    if (hasPlayed) showHeadline = true;

    // Fix iOS 100vh bug
    function setHeight() {
      if (wrapperEl) wrapperEl.style.height = `${window.innerHeight}px`;
    }
    setHeight();
    window.addEventListener('resize', setHeight);
    window.addEventListener('orientationchange', () => setTimeout(setHeight, 100));

    return () => {
      window.removeEventListener('resize', setHeight);
      window.removeEventListener('orientationchange', setHeight);
    };
  });

  async function handleCanPlay() {
    if (!videoEl) return;

    // iOS requires muted=true set in JS, not just as HTML attribute
    videoEl.muted = true;

    try {
      await videoEl.play();
      needsTap = false;
    } catch {
      // Autoplay blocked — show tap prompt instead of black screen
      needsTap = true;
    }
  }

  async function handleTap() {
    if (!videoEl) return;
    needsTap = false;
    videoEl.muted = true;
    try {
      await videoEl.play();
    } catch {
      // Still blocked — skip straight to headline
      finishVideo();
    }
  }

  function finishVideo() {
    showHeadline = true;
    localStorage.setItem(storageKey, 'true');
    dispatch('finished');
  }

  function handleEnded() {
    finishVideo();
  }

  function handleError() {
    // Video failed to load entirely — skip to headline so page isn't broken
    console.error('Video failed to load:', videoSrc);
    finishVideo();
  }
</script>

<svelte:head>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous" />
  <link
    href="https://fonts.googleapis.com/css2?family=Gravitas+One&display=swap"
    rel="stylesheet"
  />
</svelte:head>

<div class="intro-wrapper" bind:this={wrapperEl}>
  {#if shouldRenderVideo}
    <video
      bind:this={videoEl}
      class="intro-video"
      muted
      playsinline
      preload="auto"
      on:canplay={handleCanPlay}
      on:ended={handleEnded}
      on:error={handleError}
    >
      <source src={videoSrc} type="video/mp4" />
    </video>
  {/if}

  <!-- Tap-to-play overlay for iOS when autoplay is blocked -->
  {#if needsTap}
    <button class="tap-prompt" on:click={handleTap} aria-label="Tap to play">
      <div class="tap-icon">▶</div>
      <p>Tap to play</p>
    </button>
  {/if}

  <div
    class="headline-overlay"
    class:headline-visible={showHeadline}
    style={`--fade-duration: ${fadeDuration}ms;`}
  >
    <h1>{headline}</h1>
  </div>
</div>

<style>
  :global(body) {
    margin: 0;
    overscroll-behavior: none;
  }

  .intro-wrapper {
    position: relative;
    width: 100%;
    height: 100vh;
    height: 100dvh;
    overflow: hidden;
    background: black;
  }

  .intro-video {
    position: absolute;
    inset: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
  }

  @media (hover: hover) {
    .intro-video { cursor: grab; }
  }

  /* Tap-to-play prompt */
  .tap-prompt {
    position: absolute;
    inset: 0;
    z-index: 3;
    background: rgba(0, 0, 0, 0.6);
    border: none;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 1rem;
    cursor: pointer;
    color: white;
  }

  .tap-icon {
    width: 72px;
    height: 72px;
    border-radius: 50%;
    border: 3px solid white;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 1.8rem;
    padding-left: 4px; /* optical center for play triangle */
  }

  .tap-prompt p {
    font-family: sans-serif;
    font-size: 1rem;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    margin: 0;
    opacity: 0.85;
  }

  .headline-overlay {
    position: absolute;
    inset: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    opacity: 0;
    transition: opacity var(--fade-duration) ease;
    pointer-events: none;
    z-index: 2;
    background: transparent;
  }

  .headline-visible {
    opacity: 1;
  }

  h1 {
    margin: 0;
    width: 100%;
    padding: 0 1.5rem;
    text-align: center;
    text-transform: uppercase;
    color: white;
    font-family: 'Gravitas One', serif;
    font-size: clamp(2.5rem, 20vw, 12rem);
    line-height: 0.9;
    text-shadow:
      0 4px 20px rgba(0, 0, 0, 0.75),
      0 2px 6px rgba(0, 0, 0, 0.9);
  }

  @media (max-width: 480px) {
    h1 { padding: 0 1rem; }
  }
</style>