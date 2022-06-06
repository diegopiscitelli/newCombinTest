<template>
  <div id="app">
		<NavHome/>
		<MembersCreator/>
		<FooterHome/>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import FooterHome from './components/FooterHome.vue';
import MembersCreator from './components/MembersCreator.vue';
import NavHome from './components/NavHome.vue';

export default {
  name: 'App',
  components: {
		NavHome,
		MembersCreator,
    FooterHome,
  }
}
</script>

<style>
	*{
		box-sizing: border-box;
		margin: 0;
		padding: 0;
	}
	body{
		background-color: #d8dbdd;
		font-size: 15px;
	}
	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		color: #2c3e50;
		display: flex;
		justify-content: space-between;
		align-items: stretch;

		flex-direction: column;
		/* width: 100%; */
		height: 100vh;
	}
</style>
