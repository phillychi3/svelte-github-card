<script lang="ts">
	import { onMount } from 'svelte'
	import Loader from '$lib/svg/loading.svelte'
	import Star from '$lib/svg/star.svelte'
	import Fork from '$lib/svg/fork.svelte'
	import { colors } from './colors.js'

	interface Props {
		repository: string
		enable_project_image?: boolean
		enable_stars?: boolean
		enable_forks?: boolean
		enable_languages?: boolean
	}

	let { repository, enable_project_image, enable_stars, enable_forks, enable_languages }: Props = $props()
	if (enable_forks === undefined) enable_forks = true
  if (enable_project_image === undefined) enable_project_image = true
  if (enable_stars === undefined) enable_stars = true
  if (enable_languages === undefined) enable_languages = true

	let data: any = $state(null)
	let languages: any = $state(null)
	let error: any = $state(null)

	let re = new RegExp('https://github.com/(.*)/(.*)')
	let match = repository.match(re)
	if (match) {
		repository = `${match[1]}/${match[2]}`
	} else {
		error = 'Invalid repository format. Please use "owner/repo" or a full URL.'
	}

	onMount(async () => {
		try {
			const response = await fetch(`https://api.github.com/repos/${repository}`)
			if (response.ok) {
				data = await response.json()
			} else {
				error = `Error: ${response.status}`
			}

			const languagesResponse = await fetch(`https://api.github.com/repos/${repository}/languages`)
			if (languagesResponse.ok) {
				const languagesData = await languagesResponse.json()
				const total = Object.values(languagesData).reduce((sum: number, bytes: any) => sum + bytes, 0)

				languages = Object.entries(languagesData)
					.map(([name, bytes]: [string, any]) => ({
						name,
						bytes,
						percentage: ((bytes / total) * 100).toFixed(1),
						color: (colors as any)[name]?.color || '#858585'
					}))
					.sort((a, b) => b.bytes - a.bytes)
			}
		} catch (e) {
			error = e
		}
	})
</script>

<div class="flex max-w-[400px] flex-col items-center justify-center rounded-lg bg-white p-4 shadow-md">
	{#if data}
		<button class="" onclick={() => window.open(data.html_url, '_blank')}>
			<div class="{enable_project_image ? 'mr-auto items-center' : 'items-start text-left ml-2'} flex flex-row  space-x-4">
				{#if enable_project_image}
					<img src={data.owner.avatar_url} alt="avatar" class="m-2 h-20 w-20 rounded-full" />
				{/if}
				<div>
					<h1 class="text-xl font-bold">{data.name}</h1>
					<p class="text-sm text-gray-500">{data.description}</p>
				</div>
			</div>
			<div class="m-2 mr-auto flex space-x-4">
				{#if enable_stars}
					<div class="flex items-center space-x-1">
						<Star />
						<span>{data.stargazers_count}</span>
					</div>
				{/if}
				{#if enable_forks}
					<div class="flex items-center space-x-1">
						<Fork />
						<span>{data.forks_count}</span>
					</div>
				{/if}
			</div>
			{#if languages && languages.length > 0 && enable_languages}
				<div class="mb-2 w-full px-2">
					<div class="mb-2 flex h-2 overflow-hidden rounded-full bg-gray-200">
						{#each languages as lang}
							<div
								class="h-full"
								style="width: {lang.percentage}%; background-color: {lang.color};"
								title="{lang.name}: {lang.percentage}%"
							></div>
						{/each}
					</div>
					<div class="flex flex-wrap gap-2 text-xs">
						{#each languages.slice(0, 3) as lang}
							<div class="flex items-center space-x-1">
								<div class="h-3 w-3 rounded-full" style="background-color: {lang.color};"></div>
								<span class="text-gray-700">{lang.name}</span>
								<span class="text-gray-500">{lang.percentage}%</span>
							</div>
						{/each}
					</div>
				</div>
			{/if}
		</button>
	{:else if error}
		<div class="text-red-500">無法載入專案資訊: {error}</div>
	{:else}
		<div class="flex items-center justify-center">
			<Loader size="24px" color="#000" />
		</div>
	{/if}
</div>
