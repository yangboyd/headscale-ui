<script lang="ts">
	import { getDeviceRoutes, enableDeviceRoute, disableDeviceRoute } from '$lib/common/apiFunctions.svelte';
	import { Device, Route } from '$lib/common/classes';
	import { onMount } from 'svelte';
	import { alertStore } from '$lib/common/stores';

	export let device = new Device();
	let routesList = new Route();

	onMount(async () => {
		getDeviceRoutesAction();
	});

	function getDeviceRoutesAction() {
		getDeviceRoutes(device.id)
			.then((routes) => {
				routesList = routes;
			})
			.catch((error) => {
				$alertStore = error;
			});
	}

  function enableDeviceRouteAction(route: string) {
    enableDeviceRoute(device.id, route.id)
			.then((response) => {
				getDeviceRoutesAction();
			})
			.catch((error) => {
				$alertStore = error;
			});
  }

  function disableDeviceRouteAction(route: string) {
    disableDeviceRoute(device.id, route.id)
			.then((response) => {
				getDeviceRoutesAction();
			})
			.catch((error) => {
				$alertStore = error;
			});
  }
</script>

<th>Device Routes</th>
<td
	><ul class="list-disc list-inside">
		{#each routesList as route}
			<li>
				{route.prefix}
				{#if route.enabled}
					<button on:click={() => {disableDeviceRouteAction(route)}} type="button" class="btn btn-xs tooltip capitalize bg-success text-success-content mx-1" data-tip="press to disable route">active</button>
				{:else}
					<button on:click={() => {enableDeviceRouteAction(route)}} type="button" class="btn btn-xs tooltip capitalize bg-secondary text-secondary-content mx-1" data-tip="press to enable route">pending</button>
				{/if}
			</li>
		{/each}
	</ul></td
>
