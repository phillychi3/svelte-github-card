# Svelte GitHub Card

## 安裝

```bash
npm install svelte-github-card
# 或
pnpm add svelte-github-card
# 或
yarn add svelte-github-card
```

## 使用方法

```svelte
<script>
	import { Github } from 'svelte-github-card'
</script>

<Github repository="sveltejs/kit" />

<Github repository="https://github.com/phillychi3/MyGo-Meme" />

<Github
	repository="sveltejs/kit"
	enable_project_image={false}
	enable_languages={true}
	enable_stars={true}
	enable_forks={true}
/>
```

colors from https://github.com/ozh/github-colors
