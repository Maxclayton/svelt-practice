
<script>
    import {fly} from 'svelte/transition';
    import CircleProgressBar from "./CircleProgressBar.svelte"; 
    import './main.css';
    import { onMount } from 'svelte';

    let colors = ['blue', 'green', 'red']
    var count = 0;
    let bars = [
    { delay: 250, duration: 300, x: 100, y: 500, opacity: 0.5, progress:  Math.floor(Math.random() * 100), color: colors[count] },
    // { delay: 350, duration: 300, x: 100, y: 500, opacity: 0.5, progress:  Math.floor(Math.random() * 100), color: colors[count + 1] },
    // { delay: 450, duration: 300, x: 100, y: 300, opacity: 0.5, progress:  Math.floor(Math.random() * 100), color: colors[count + 2] }
  ];
  let showBars = false; 


  onMount(() => {
  setTimeout(() => {
    showBars = true;
  }, 250);
});

   // Function to determine the color based on progress value
   function determineColor(progress) {
        if (progress >= 75) {
            return 'green';
        } else if (progress >= 50) {
            return 'orange';
        } else {
            return 'red';
        }
    }

    // Update bar colors based on progress
    $: {
        bars.forEach(bar => {
            bar.color = determineColor(bar.progress);
        });
    }




</script>

<main>
  <div class="graph-container">
    {#each bars as bar (bar.progress)}
      {#if showBars}
      <div class='circle-container' >
        <CircleProgressBar  progress={bar.progress / 100} color={bar.color}  />
          <input
          id="progress-value"
          type="number"
          max="100"
          min="1"
          bind:value={bar.progress}
          on:input={(event) => {
            // Ensure the progress value is within the valid range
            const newValue = Math.min(100, Math.max(1, event.target.value));
            bar.progress = newValue;
          }}
        >
       </div>

      {/if}
    {/each}
  </div>
</main>

<style>

  .circle-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 10px;
}

#progress-value {
    height: 20px;
    border-radius: 3px;
    width: 100px;
}

    	/* Graph Container Styles */
	.graph-container {
		display: flex;
		justify-content: space-around;
		width: 100%;
	}

</style>