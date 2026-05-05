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

  onMount(() => {
    const hasPlayed = localStorage.getItem(storageKey);

    if (!hasPlayed) {
      shouldRenderVideo = true;
    } else {
      shouldRenderVideo = true;
      showHeadline = true;
    }

    // Fix iOS 100vh bug — use actual window height instead of CSS vh
    function setHeight() {
      if (wrapperEl) {
        wrapperEl.style.height = `${window.innerHeight}px`;
      }
    }

    setHeight();
    window.addEventListener('resize', setHeight);
    // Also fire on orientation change for phones
    window.addEventListener('orientationchange', () => {
      setTimeout(setHeight, 100);
    });

    return () => {
      window.removeEventListener('resize', setHeight);
      window.removeEventListener('orientationchange', setHeight);
    };
  });

  async function handleCanPlay() {
    if (videoEl) {
      try {
        // Ensure muted before play — required for autoplay on iOS
        videoEl.muted = true;
        await videoEl.play();
      } catch (error) {
        console.error('Autoplay failed:', error);
        showHeadline = true;
        dispatch('finished');
      }
    }
  }

  function handleEnded() {
    showHeadline = true;
    localStorage.setItem(storageKey, 'true');
    dispatch('finished');
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
    >
      <source src={videoSrc} type="video/mp4" />
      Your browser does not support the video tag.
    </video>
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
    /* Prevent iOS bounce scroll revealing background */
    overscroll-behavior: none;
  }

  .intro-wrapper {
    position: relative;
    width: 100%;
    /* Fallback for browsers that don't support dvh */
    height: 100vh;
    /* dvh = dynamic viewport height — accounts for iOS Safari browser chrome */
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
    /* Remove grab cursor on mobile — not meaningful on touch */
    cursor: default;
  }

  @media (hover: hover) {
    /* Only show grab cursor on devices that have a real pointer */
    .intro-video {
      cursor: grab;
    }
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
    /* clamp: min 2.5rem on tiny phones, scales with viewport, max 20vw on desktop */
    font-size: clamp(2.5rem, 20vw, 12rem);
    line-height: 0.9;
    text-shadow:
      0 4px 20px rgba(0, 0, 0, 0.75),
      0 2px 6px rgba(0, 0, 0, 0.9);
  }

  /* Portrait phones — tighten padding */
  @media (max-width: 480px) {
    h1 {
      padding: 0 1rem;
    }
  }
</style>