<script lang="ts">
  import { onMount } from 'svelte';
  import { assets } from '$app/paths';

  export let src = `${assets}/TerryDunningquote_1.mp4`;
  export let quote = 'So I mean the numbers are getting beefed up. They definitely paying for their numbers. And you can get over 250,000 views for I think its 249. Thats nothing for Dana White when hes kind of build this motion up. But from what I hear, he building it up to dump it. He realized theres not a lot of money into this, but not as much as he thought.';
  export let attribution = 'Terry Dunning';
  export let size = 'large';

  let videoEl: HTMLVideoElement;
  let playing = false;
  let muted = false;

  function togglePlay() {
    if (videoEl.paused) {
      videoEl.play();
      playing = true;
    } else {
      videoEl.pause();
      playing = false;
    }
  }

  function toggleMute() {
    videoEl.muted = !videoEl.muted;
    muted = videoEl.muted;
  }

  onMount(() => {
    videoEl.addEventListener('ended', () => (playing = false));
  });
</script>

<div class="vq-wrapper" data-size={size}>
  <!-- Left: Video -->
  <div class="vq-video-col">
    <div class="vq-video-container">
      <!-- svelte-ignore a11y-media-has-caption -->
      <video bind:this={videoEl} {src} playsinline class="vq-video"></video>

      <!-- Custom controls bar -->
      <div class="vq-controls">
        <button class="vq-btn" on:click={togglePlay} aria-label={playing ? 'Pause' : 'Play'}>
          {#if playing}
            <svg viewBox="0 0 24 24" fill="currentColor" width="20" height="20">
              <rect x="5" y="4" width="4" height="16" rx="1"/>
              <rect x="15" y="4" width="4" height="16" rx="1"/>
            </svg>
          {:else}
            <svg viewBox="0 0 24 24" fill="currentColor" width="20" height="20">
              <path d="M6 4l14 8-14 8V4z"/>
            </svg>
          {/if}
        </button>

        <button class="vq-btn" on:click={toggleMute} aria-label={muted ? 'Unmute' : 'Mute'}>
          {#if muted}
            <svg viewBox="0 0 24 24" fill="currentColor" width="20" height="20">
              <path d="M3 9v6h4l5 5V4L7 9H3zm13.5 3A4.5 4.5 0 0 0 14 7.97v8.05c1.48-.73 2.5-2.25 2.5-4.02zM14 3.23v2.06c2.89.86 5 3.54 5 6.71s-2.11 5.85-5 6.71v2.06c4.01-.91 7-4.49 7-8.77 0-4.28-2.99-7.86-7-8.77z"/>
              <line x1="1" y1="1" x2="23" y2="23" stroke="currentColor" stroke-width="2"/>
            </svg>
          {:else}
            <svg viewBox="0 0 24 24" fill="currentColor" width="20" height="20">
              <path d="M3 9v6h4l5 5V4L7 9H3zm13.5 3A4.5 4.5 0 0 0 14 7.97v8.05c1.48-.73 2.5-2.25 2.5-4.02zM14 3.23v2.06c2.89.86 5 3.54 5 6.71s-2.11 5.85-5 6.71v2.06c4.01-.91 7-4.49 7-8.77 0-4.28-2.99-7.86-7-8.77z"/>
            </svg>
          {/if}
        </button>
      </div>
    </div>
  </div>

  <!-- Right: Pull quote -->
  <div class="vq-quote-col">
    <blockquote class="vq-pullquote">
      <span class="vq-quotemark">&ldquo;</span>
      <p class="vq-quote-text">{quote}</p>
      {#if attribution}
        <cite class="vq-attribution">— {attribution}</cite>
      {/if}
    </blockquote>
  </div>
</div>

<style>
  .vq-wrapper {
    display: flex;
    flex-direction: row;
    align-items: stretch;
    gap: 0;
    margin: 2rem auto;
    width: 100%;
    overflow: hidden;
    border-radius: 8px;
    box-shadow: 0 4px 24px rgba(0, 0, 0, 0.1);
  }

  .vq-wrapper[data-size='fit']   { max-width: 680px; }
  .vq-wrapper[data-size='large'] { max-width: 1100px; }
  .vq-wrapper[data-size='full']  { max-width: 100%; border-radius: 0; }

  .vq-video-col {
    flex: 1 1 55%;
    position: relative;
    background: #000;
    min-height: 250px;
  }

  .vq-video-container {
    position: relative;
    width: 100%;
    height: 100%;
  }

  .vq-video {
    width: 100%;
    height: 100%;
    display: block;
    object-fit: cover;
  }

  .vq-controls {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    display: flex;
    gap: 4px;
    padding: 10px 12px;
    background: linear-gradient(transparent, rgba(0, 0, 0, 0.6));
  }

  .vq-btn {
    display: flex;
    align-items: center;
    justify-content: center;
    background: rgba(255, 255, 255, 0.15);
    border: 1px solid rgba(255, 255, 255, 0.25);
    color: #fff;
    border-radius: 4px;
    width: 36px;
    height: 36px;
    cursor: pointer;
    transition: background 0.2s;
    padding: 0;
    backdrop-filter: blur(4px);
  }

  .vq-btn:hover {
    background: rgba(255, 255, 255, 0.3);
  }

  .vq-quote-col {
    flex: 1 1 45%;
    display: flex;
    align-items: center;
    justify-content: center;
    background: #1a1a1a;
    padding: 3rem 2.5rem;
  }

  .vq-pullquote {
    margin: 0;
    padding: 0;
    border: none;
    text-align: left;
  }

  .vq-quotemark {
    display: block;
    font-size: 5rem;
    line-height: 1;
    color: #fff;
    opacity: 0.2;
    font-family: Georgia, serif;
    margin-bottom: -1.5rem;
    margin-left: -0.25rem;
  }

  .vq-quote-text {
    font-family: Georgia, 'Times New Roman', serif;
    font-size: 1.4rem;
    line-height: 1.65;
    color: #f0f0f0;
    margin: 0 0 1.25rem;
    font-style: italic;
  }

  .vq-attribution {
    display: block;
    font-family: sans-serif;
    font-size: 0.8rem;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    color: #888;
    font-style: normal;
  }

  @media (max-width: 640px) {
    .vq-wrapper {
      flex-direction: column;
    }

    .vq-video-col {
      flex: none;
      min-height: 220px;
    }

    .vq-quote-col {
      flex: none;
      padding: 2rem 1.5rem;
    }

    .vq-quote-text {
      font-size: 1.15rem;
    }
  }
</style>