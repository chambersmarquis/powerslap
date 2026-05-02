<script lang="ts">
  export let quote: string = '';
  export let cite: string = '';
</script>

{#if quote}
    <div class="contract-quote-wrapper">
        <blockquote>
            <p>{quote}</p>
            {#if cite}
            <cite>{cite}</cite>
            {/if}
        </blockquote>
    </div>
{/if}

<style>
  /* Scoped to this component only — will not bleed into the rest of the page */

  .contract-quote-wrapper {
    --orange: #e85d04;
    --gray-70: #4a4a4a;
    --gray-50: #7a7a7a;
  }

  .contract-quote-wrapper blockquote {
    margin: 2rem 0;
    padding: 1.2rem 0 1.2rem 1.5rem;
    border-left: 2px solid var(--orange);
  }

  .contract-quote-wrapper blockquote p {
    font-family: 'Bitter', Georgia, serif;
    font-style: italic;
    font-size: 1.05rem;
    font-weight: 400;
    line-height: 1.6;
    color: var(--gray-70);
    margin-bottom: 0.65rem;
  }

  .contract-quote-wrapper blockquote cite {
    font-size: 0.72rem;
    font-weight: 500;
    letter-spacing: 0.04em;
    color: var(--gray-50);
    font-style: normal;
  }
</style>