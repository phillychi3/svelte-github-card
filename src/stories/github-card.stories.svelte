<script module>
	import { defineMeta } from '@storybook/addon-svelte-csf'
	import Github from '../lib/github-card/github.svelte'

	const { Story } = defineMeta({
		title: 'Components/Github Card',
		component: Github,
		argTypes: {
			repository: { control: 'text' }
		},
		args: {
			repository: 'sveltejs/svelte'
		},
		tags: ['autodocs']
	})
</script>

<Story name="Default" />

<Story
	name="With Full URL"
	args={{
		repository: 'https://github.com/sveltejs/kit'
	}}
/>

<Story
	name="Invalid Repository"
	args={{
		repository: 'not-a-real/repository'
	}}
/>
