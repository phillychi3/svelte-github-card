<script lang="ts">
	import { onMount } from 'svelte'
	import Loader from '$lib/svg/loading.svelte'
	import Star from '$lib/svg/star.svelte'
	import Fork from '$lib/svg/fork.svelte'

	interface Props {
		repository: string
	}

	let { repository }: Props = $props()
	let data: any = $state(null)
	let error: any = $state(null)

	let re = new RegExp('https://github.com/(.*)/(.*)')
	let match = repository.match(re)
	if (match) {
		repository = `${match[1]}/${match[2]}`
	}

	onMount(async () => {
		try {
			const response = await fetch(`https://api.github.com/repos/${repository}`)
			if (response.ok) {
				data = await response.json()
			} else {
				error = `Error: ${response.status}`
			}
		} catch (e) {
			error = e
		}
	})
</script>

<div class="flex max-w-[400px] flex-col items-center justify-center rounded-lg bg-white p-4 shadow-md">
	{#if data}
		<div class="mr-auto flex flex-row items-center space-x-4">
			<img src={data.owner.avatar_url} alt="avatar" class="m-2 h-20 w-20 rounded-full" />
			<div>
				<h1 class="text-lg font-bold">{data.name}</h1>
				<p class="text-sm text-gray-500">{data.description}</p>
			</div>
		</div>
		<div class="m-2 mr-auto flex space-x-4">
			<div class="flex items-center space-x-1">
				<Star />
				<span>{data.stargazers_count}</span>
			</div>
			<div class="flex items-center space-x-1">
				<Fork />
				<span>{data.forks_count}</span>
			</div>
		</div>
	{:else if error}
		<div class="text-red-500">無法載入專案資訊: {error}</div>
	{:else}
		<div class="flex items-center justify-center">
			<Loader size="24px" color="#000" />
		</div>
	{/if}
</div>
