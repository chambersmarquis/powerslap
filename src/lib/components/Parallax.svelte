<script>
  /**
   * Parallax component — mirrors the W3Schools parallax demo layout.
   * Alternates between scrollable text sections and parallax image bands.
   *
   * Props:
   *   sections — array of content sections between the parallax bands
   *   parallaxImages — array of image paths for the parallax bands (from /gallery or any path)
   */

  export let sections = [
    {
      title: 'Parallax Scrolling',
      body: `Parallax scrolling is a web site trend where the background content is
             moved at a different speed than the foreground content while scrolling.
             Click and scroll up and down to really get the feeling of how
             parallax scrolling works.`,
    },
    {
      title: 'Less Height',
      body: `Scroll up and down to really get the feeling of how parallax scrolling works.`,
      short: true,
    },
    {
      title: 'Scroll Up',
      body: `Scroll up and down to really get the feeling of how parallax scrolling works.`,
      short: true,
    },
  ];

  export let parallaxImages = [
    '/contract1.png',
    '/contract2.png',
    '/contract3.png',
  ];
</script>

<div class="parallax-page full-bleed ">
  {#each sections as section, i (i)}
    <!-- Text content section -->
    <div class="content-section" class:short={section.short}>
      {#if section.title}
        <h2>{section.title}</h2>
      {/if}
      {#if section.body}
        <p>{section.body}</p>
      {/if}
    </div>

    <!-- Parallax image band -->
    {#if parallaxImages[i]}
      <div
        class="parallax-band"
        style="background-image: url('{parallaxImages[i]}')"
        aria-hidden="true"
      ></div>
    {/if}
  {/each}
</div>

<style>
  .parallax-page {
    font-family: 'Times New Roman', serif;
    font-size: 17px;
    color: #333;
  }

  /* Text content sections */
  .content-section {
    padding: 80px 80px;
    background: #fff;
    text-align: center;
    min-height: 300px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .content-section.short {
    min-height: 160px;
    padding: 40px 80px;
  }

  .content-section h2 {
    font-size: 2rem;
    font-weight: 400;
    margin: 0 0 1rem;
    letter-spacing: 0.05em;
    text-transform: uppercase;
  }

  .content-section p {
    max-width: 680px;
    line-height: 1.8;
    margin: 0;
    color: #555;
  }

  /* Parallax bands */
  .parallax-band {
    /* The magic: fixed attachment makes the background scroll at a different
       speed than the page, creating the parallax effect */
    background-attachment: fixed;
    background-position: center;
    background-repeat: no-repeat;
    background-size: contain;

    min-height: 400px;
    width: 100%;
  }

  /* Smaller band variant — mirrors the "less height" demo section */
  .parallax-band:nth-child(even) {
    min-height: 100vh;
  }

  @media (max-width: 768px) {
    .content-section {
      padding: 60px 24px;
    }

    /* background-attachment: fixed doesn't work well on iOS Safari,
       fall back to scroll so it doesn't break on mobile */
    .parallax-band {
      background-attachment: scroll;
      min-height: 260px;
    }
  }
</style>