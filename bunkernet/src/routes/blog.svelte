<script context="module">
	export async function load({ url, fetch }) {
		const newurl = `/blog/all.json`;
		const res = await fetch(newurl);

		if (res.ok) {
			const { posts } = await res.json();

			return {
				props: {
					posts
				}
			};
		}
		return {
			status: res.status,
			error: new Error(`Could not load ${url}`)
		};
	}
</script>

<script>
	import Typewriter from '../components/typewriter.svelte';
	import PostPreview from '../components/post-preview-small.svelte';

	export let posts;
</script>

<!--Blog--->

<div id="Posts" class="flex-col justify-around py-16 gap-8 mx-auto">
	<div class="h-full flex flex-col justify-evenly">
		<Typewriter
			clazz={'font-semibold text-2xl md:text-5xl'}
			alt_color={'text-cyan dark:text-cyan'}
			command={'ls -al ~/Posts'}
			result={`total ${posts.length}`}
		/>
	</div>
</div>

<!--List of posts-->

<div class="grid gap-4 sm:px-16 py-16">
	{#each posts as post}
		<PostPreview {post} />
	{/each}
</div>
