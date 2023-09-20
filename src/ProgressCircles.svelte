
<script>
  import {fly} from 'svelte/transition';
  import CircleProgressBar from "./CircleProgressBar.svelte"; 
  import './main.css';
  import { onMount } from 'svelte';
    import Fa from 'svelte-fa';
    import { faPlus, faMinus } from '@fortawesome/free-solid-svg-icons';
  let colors = ['blue', 'green', 'red']
  var count = 0;
  let bars = [
  { delay: 250, duration: 300, x: 100, y: 500, opacity: 0.5, progress: Math.floor(Math.random() * 100), color: colors[count] },
  // { delay: 350, duration: 300, x: 100, y: 500, opacity: 0.5, progress: Math.random(), color: colors[count + 1] },
  // { delay: 450, duration: 300, x: 100, y: 300, opacity: 0.5, progress: Math.random(), color: colors[count + 2] }
];

   function determineColor(progress) {
        if (progress >= 75) {
            return 'green';
        } else if (progress >= 50) {
            return 'orange';
        } else {
            return 'red';
        }
    }
    $: {
        bars.forEach(bar => {
            bar.color = determineColor(bar.progress);
        });
    }



</script>
<main>
<div class="graph-container">
    {#each bars as bar (bar.progress)}
      <div class='circle-container' >
        <CircleProgressBar  progress={bar.progress / 100} color={bar.color}  />
        <div class='progress-buttons'>
          <button on:click={() => (bar.progress = bar.progress + 1)}><Fa icon={faPlus}/></button>
          <div class='progress'>{bar.progress}</div>
          <button on:click={() => (bar.progress = bar.progress - 1)}><Fa icon={faMinus}/></button>

        </div>


      </div>

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
.progress-buttons {
  display: flex;
  flex-direction: row;
  gap: 15px;
  align-items: center;
  justify-content: center;
}

.progress-buttons button {
  margin: 0;
  outline: none;
    border-radius: 100%;
    width: 100px;
    height: 100px;
    border: none;
    font-size: 30px;
}

.progress {
  font-size: 25px;
}
    /* Graph Container Styles */
.graph-container {
  display: flex;
  justify-content: space-around;
  width: 100%;
}
</style>