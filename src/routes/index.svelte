<script context="module" lang="ts">
	import type { Load } from '@sveltejs/kit';
	import { allMethods } from '$lib/stores';
	export const prerender = true;

	export const load: Load = async ({ fetch }) => {
		const res = await fetch('methods.json');

		if (res.ok) {
			const result = await res.json();

			const methodsArray = result.methodsArray;
			allMethods.set(methodsArray);

			return {
				props: {
					methodsArray
				}
			};
		}

		const { message } = await res.json();

		return {
			error: new Error('[index.svelte]: ', message)
		};
	};
</script>

<script lang="ts">
	import type { Method } from '$lib/types';
	import MethodList from '$lib/components/method/MethodList.svelte';
	import CategoryTab from '$lib/components/tabs/CategoryTab.svelte';

	export let methodsArray: Array<Method>;
</script>

<svelte:head>
	<title>ICT Research Methods — Research Methods for Design-Oriented Research in ICT</title>
</svelte:head>

<h1 class="site-title">Methods</h1>
<CategoryTab />
<MethodList {methodsArray} />

<style lang="scss">
	h1 {
		font-weight: 500;
		font-size: 1.25em;
		margin: 1em;
		width: 100%;
		text-align: center;

		@include desktop {
			text-align: left;
			margin-left: 0;
		}
	}
</style>
