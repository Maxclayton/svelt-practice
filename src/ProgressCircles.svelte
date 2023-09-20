
<script>
    import {fly} from 'svelte/transition';
    import CircleProgressBar from "./CircleProgressBar.svelte"; 
    import './main.css';
    import { onMount } from 'svelte';

    let colors = ['blue', 'green', 'red']
    var count = 0;
    let bars = [
    { delay: 250, duration: 300, x: 100, y: 500, opacity: 0.5, progress: Math.random(), color: colors[count] },
    { delay: 350, duration: 300, x: 100, y: 500, opacity: 0.5, progress: Math.random(), color: colors[count + 1] },
    { delay: 450, duration: 300, x: 100, y: 300, opacity: 0.5, progress: Math.random(), color: colors[count + 2] }
  ];

  let showBars = false; 

  onMount(() => {
      setTimeout(() => {
          showBars = true;
      }, 250);
  });


</script>

<main>
  <div class="graph-container">
    {#each bars as bar (bar.progress)}
      {#if showBars}
        <div 
          transition:fly={{ delay: bar.delay, duration: bar.duration, x: bar.x, y: bar.y, opacity: bar.opacity }}>
          <CircleProgressBar  progress={bar.progress} color={bar.color}  />
        </div>
      {/if}
    {/each}
  </div>
</main>

<style>

    	/* Graph Container Styles */
	.graph-container {
		display: flex;
		justify-content: space-around;
		width: 100%;
	}

</style>