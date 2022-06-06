<template>
<!-- Container used to position the loader in the horizontally and vertically to the center -->
<div class="circular-loader-container">
	<div v-if="show" class="circular-loader"></div>
</div>
</template>

<script>
export default {
	name: 'CircularLoader',
	props: {
		show: {
			type: Boolean,
			default: false
		}
	}
};
</script>

<style scoped>
	.circular-loader-container {
		width: 100%;
		height: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	.circular-loader {
			border: 16px solid #eeeded;
			border-top: 16px solid #578ddd;
			border-radius: 50%;
			width: 80px;
			height: 80px;
			animation: spin 2s linear infinite;
		}
		@keyframes spin {
			0% { transform: rotate(0deg); }
			100% { transform: rotate(360deg); }
		}
</style>