<script>
	export let size = '24px'
	export let color = '#000'
</script>

<div class="circle-loader" style="--size: {size}; --color: {color}">
	<div></div>
	<div></div>
	<div></div>
	<div></div>
	<div></div>
	<div></div>
	<div></div>
	<div></div>
</div>

<style>
	.circle-loader {
		display: inline-flex;
		position: relative;
		width: var(--size);
		height: var(--size);
	}

	.circle-loader div {
		transform-origin: 50% 100%;
		animation: rotate 0.6s linear infinite;
	}

	.circle-loader div::after {
		content: ' ';
		display: block;
		position: absolute;
		background-color: var(--color);
		top: calc(var(--size) * 0.33);
		left: calc(var(--size) * 0.33);
		width: calc(var(--size) * 0.3);
		height: calc(var(--size) * 0.3);
		border-radius: 100%;
	}

	.circle-loader div:nth-child(1) {
		transform: rotate(0deg);
		animation-delay: -0.525s;
	}
	.circle-loader div:nth-child(2) {
		transform: rotate(45deg);
		animation-delay: -0.45s;
	}
	.circle-loader div:nth-child(3) {
		transform: rotate(90deg);
		animation-delay: -0.375s;
	}
	.circle-loader div:nth-child(4) {
		transform: rotate(135deg);
		animation-delay: -0.3s;
	}
	.circle-loader div:nth-child(5) {
		transform: rotate(190deg);
		animation-delay: -0.225s;
	}
	.circle-loader div:nth-child(6) {
		transform: rotate(235deg);
		animation-delay: -0.15s;
	}
	.circle-loader div:nth-child(7) {
		transform: rotate(280deg);
		animation-delay: -0.075s;
	}
	.circle-loader div:nth-child(8) {
		transform: rotate(320deg);
		animation-delay: 0s;
	}

	@keyframes rotate {
		0% {
			opacity: 1;
		}
		100% {
			opacity: 0;
		}
	}
</style>
