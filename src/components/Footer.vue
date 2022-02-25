<script setup lang="ts">
</script>

<template>
	<footer class="mt-auto">
		<div class="bg-dark text-light mt-3">
			<div class="container-fluid p-3">
				<h1 class="text-center">{{ title + ": " + date }}</h1>
			</div>
		</div>
	</footer>
</template>

<script lang="ts">
export default {
	props: ['title'],
	name: 'App',
	data() {
		return {

		}
	},
	computed: {
		date() {
			let now = new Date();
			let year = String(now.getFullYear());
			let month = String(now.getMonth() + 1);
			let day = String(now.getDate());

			if (month.toString().length == 1) {
				month = '0' + month;
			}
			if (day.toString().length == 1) {
				day = '0' + day;
			}

			return day + '.' + month + '.' + year;
		}
	},
	watch: {

	}
}
</script>

<style scoped>
</style>
