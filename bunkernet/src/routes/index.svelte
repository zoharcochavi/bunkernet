<script context="module">
	import { Octokit } from '@octokit/rest';
	import dotenv from 'dotenv/config';

	export async function load() {
		const include = [
			'dotfiles',
			'BattleDucks',
			'gopy',
			'wikiscraper',
			'dotfield',
			'music-bot-family'
		];

		let token = process.env['GH_TOKEN'];
		const octo = new Octokit({ auth: token });
		const { data } = await octo.rest.repos.listForAuthenticatedUser();

		let repos = data.filter(
			(repo) =>
				repo.owner.login === 'zoharcochavi' &&
				repo.description != null &&
				include.includes(repo.name)
		);
		return {
			props: {
				repos
			}
		};
	}
</script>

<script>
	import Typewriter from '../components/typewriter.svelte';
	import SlidingRepo from '../components/sliding-repo.svelte';

	export let lastRepo;
	export let repos;

	lastRepo = repos[repos.length - 1];
</script>

<!--Home--->

<div
	id="Home"
	class="min-h-1/2 max-h-4/5 flex flex-col justify-between border-b-2 sm:gap-y-32 gap-y-16 border-green pt-16"
>
	<Typewriter
		clazz={'font-semibold text-2xl md:text-5xl'}
		alt_color={'text-cyan dark:text-cyan'}
		command={'echo "Hi, my name is $(whoami)\\!"'}
		result={'Hi, my name is Zohar!'}
	/>
	<p class="sm:px-16 lg:w-4/5">
		I’m a Computer Science student who is interested in pretty much everything (for better or for
		worse)! This is the place where I collect most interesting things I come across, so feel free to
		have a look and see if there is anything that catches your eye!
	</p>
	<div class="w-full lg:px-16 pb-8 flex flex-row-reverse gap-4">
		<a target="_blank" href="https://github.com/zoharcochavi">
			<img
				class="transition transform hover:scale-100 w-8 sm:w-12"
				src="/images/github-logo/PNG/GitHub-Mark-Light-120px-plus.png"
				alt="Github profile of Zohar Cochavi"
			/>
		</a>
		<a target="_blank" href="https://www.linkedin.com/in/zoharcochavi/">
			<img
				class="transition transform hover:scale-100 w-8 sm:w-12"
				src="/linkedin_logo.png"
				alt="Linkedin profile of Zohar Cochavi"
			/>
		</a>
	</div>
</div>

<!--Projects--->

<div class="flex flex-col py-8 sm:py-16">
	<Typewriter
		alt_color={'text-cyan dark:text-cyan'}
		clazz={'font-semibold text-2xl md:text-5xl py-8'}
		command={'ls -l ~/Projects'}
		result={`total ${repos.length}`}
		idle={false}
	/>
	<div class="flex flex-col lg:px-16">
		{#each repos.slice(0, -1) as repo}
			<div
				class="flex gap-16 sm:odd:flex-row sm:even:flex-row-reverse flex-col border-b-4 border-dashed dark:border-graycode-dark border-graycode-light py-16"
			>
				<img class="w-4/5 mx-auto" src={`${repo.name}.png`} alt={repo.name} />
				<SlidingRepo {repo} />
			</div>
		{/each}
		{#each repos.slice(-1) as repo}
			<div class="flex gap-16 sm:odd:flex-row sm:even:flex-row-reverse flex-col py-16">
				<img class="w-4/5 mx-auto" src={`${repo.name}.png`} alt={repo.name} />
				<SlidingRepo {repo} />
			</div>
		{/each}
	</div>
</div>
