<script lang="ts">
  import rawBlocks from '$lib/doc.blocks.json';
  import DocRenderer from '$lib/components/DocRenderer.svelte';
  import type { Block } from '$lib/components/DocRenderer.svelte';
  import IntroHero from '$lib/components/IntroHero.svelte';

  const blocks = rawBlocks as Block[];
</script>



<!-- 
Hard-code custom code that should appear BEFORE Google Doc here. 
This code is in +page.svelte and can be manually coded using HTML or Svelte tags.
-->
<IntroHero/>
<div class="headline-wrapper">

  <!-- Main article headline
       CUSTOMIZE: Update this text to your actual headline
       CUSTOMIZE: Change <h1> to <h2> if you need a subheading instead -->
  <center> <h1 class="headline">The fighters who take a hit to survive</h1> </center>

</div>


<!-- DO NOT EDIT (except to modify classes). This will become code imported from Google Doc. -->
<div class="container">
  <div class="row justify-content-center">
    <div class="col-12 col-sm-10 col-lg-8 col-xxl-6">
      <DocRenderer {blocks} />
    </div> 
  </div>
</div>


<!-- Hard-code any custom code that should appear AFTER Google Doc below here. -->
<footer class="container-fluid bg-dark text-white p-5">
  <p class="text-center">UC Berkeley Journalism </p>
</footer>