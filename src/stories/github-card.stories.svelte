<script module>
	import { defineMeta } from '@storybook/addon-svelte-csf'
	import Github from '../lib/github-card/github.svelte'

	const { Story } = defineMeta({
		title: 'Components/Github Card',
		component: Github,
		argTypes: {
			repository: { control: 'text' },
      enable_project_image: { control: 'boolean' },
      enable_stars: { control: 'boolean' },
      enable_forks: { control: 'boolean' },
      enable_languages: { control: 'boolean' }
		},
		args: {
			repository: 'sveltejs/svelte',
      enable_project_image: true,
      enable_stars: true,
      enable_forks: true,
      enable_languages: true
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
