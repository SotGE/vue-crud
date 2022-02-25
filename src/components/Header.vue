<script setup lang="ts">
import { RouterLink } from 'vue-router'
</script>

<template>
	<header>
		<div class="bg-dark text-light mb-3">
			<div class="container-fluid p-3">
				<h1 class="text-center">{{ title }}</h1>
			</div>
		</div>
		<div class="container-fluid mb-3">
			<ul class="nav nav-tabs">
				<li class="nav-item">
					<RouterLink
						class="nav-link"
						tag="a"
						active-class="active"
						exact-active-class="exact-active"
						to="/"
					>Главная</RouterLink>
				</li>
				<li class="nav-item">
					<RouterLink
						class="nav-link"
						tag="a"
						active-class="active"
						exact-active-class="exact-active"
						to="/about"
					>О платформе</RouterLink>
				</li>
			</ul>
		</div>
	</header>
</template>

<script lang="ts">
export default {
	props: ['title']
}
</script>

<style scoped>
</style>
