<template>
	<router-view v-slot="{ Component }">
		<transition name="loadPage">
			<component :is="Component" />
		</transition>
	</router-view>
</template>

<style scoped>
.loadPage-enter-from {
	opacity: 0;
	transform: translateY(50px);
}
.loadPage-enter-to {
	opacity: 1;
	transform: translateY(0px);
}
.loadPage-enter-active {
	transition: all 3s;
}
</style>
