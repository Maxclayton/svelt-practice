<script>
	export let progress
	const angle = 360 * progress
  export let color = 'blue';
	let circleText = 'Progress';


	
	// Adapt the logic according to the approach
	const	background = `radial-gradient(white 50%, transparent 51%),
    conic-gradient(transparent 0deg ${angle}deg, gainsboro ${angle}deg 360deg),
    conic-gradient(${color} 0deg, ${color} 90deg, ${color} 180deg, ${color});`;
	
	$: cssVarStyles = `--background:${background}`
  progress = progress * 100;

  $: {
    if (progress === 100) {
      circleText = '🤩';
    } else if (progress > 100) {
      circleText = '🤯';

    } else if (progress >= 75) {
      circleText = '🙂';
    }  else if (progress >= 50) {
      circleText = '🤔';
    } else {
      circleText = '🤮';
    }
  }

</script>

<style>
#progress-circle {
  background: var(--background);
  border-radius: 50%;
  width: 200px;
  height: 200px;
  display: flex;
  align-items: center;
  justify-content: center;
	transition: all 500ms ease-in;
	will-change: transform;
  font-size: 100px;
}
</style>

<div id="progress-circle" style="{cssVarStyles}">
  {circleText}
</div>