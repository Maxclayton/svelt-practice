<script>
	export let progress
	const angle = 360 * progress
  export let color = 'blue';

	
	// Adapt the logic according to the approach
	const	background = `radial-gradient(white 50%, transparent 51%),
    conic-gradient(transparent 0deg ${angle}deg, gainsboro ${angle}deg 360deg),
    conic-gradient(${color} 0deg, ${color} 90deg, ${color} 180deg, ${color});`;
	
	$: cssVarStyles = `--background:${background}`
</script>

<style>
#progress-circle {
  background: var(--background);
  border-radius: 50%;
  width: 120px;
  height: 120px;
  display: flex;
  align-items: center;
    justify-content: center;
	transition: all 500ms ease-in;
	will-change: transform;
}
</style>

<div id="progress-circle" style="{cssVarStyles}">Progress</div>
