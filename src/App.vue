<template>
	<div id="app"><GameApp /></div>
</template>

<script>
import GameApp from '@/components/GameApp'
const message = '传递给子组件的消息'
export default {
	name: 'App',
	components: { GameApp },
	methods: {},
	data() {
		return {
			message,
		}
	},
}
</script>

<style>
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
	margin-top: 60px;
}
</style>
