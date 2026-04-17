<script>
  import { onMount, createEventDispatcher } from 'svelte';

  export let videoSrc = '/headervideo.mp4';
  export let headline = 'POWER SLAP';
  export let storageKey = 'intro-video-played';
  export let fadeDuration = 1500;

  const dispatch = createEventDispatcher();

  let videoEl;
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
  });

  async function handleCanPlay() {
    
    if ( videoEl) {
      try {
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

<div class="intro-wrapper">
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
  }

  .intro-wrapper {
    position: relative;
    width: 100vw;
    height: 100vh;
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
    font-size:  20vw;
    line-height: 0.9;
    text-shadow:
      0 4px 20px rgba(0, 0, 0, 0.75),
      0 2px 6px rgba(0, 0, 0, 0.9);
  }
</style>