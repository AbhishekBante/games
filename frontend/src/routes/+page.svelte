<!--
SPDX-FileCopyrightText: 2023 Marlon W (Mawoka)

SPDX-License-Identifier: MPL-2.0
-->

<script lang="ts">
	import { navbarVisible } from '$lib/stores';
	import { getLocalization } from '$lib/i18n';
	import Footer from '$lib/footer.svelte';
	import WebPOpenGraph from '$lib/assets/landing/opengraph-home.webp';
	import JpgOpenGraph from '$lib/assets/landing/opengraph-home.jpg';
	import login_icon from "$lib/assets/all/login_icon.webp";
	import { fly, fade } from 'svelte/transition';
	import hand_click_icon from "$lib/assets/all/hand_click_icon.svg";
	import hand_click_icon_dark from "$lib/assets/all/hand_click_icon_dark.svg";
	/*	import LandingPromo from '$lib/landing/landing-promo.svelte';*/

	import { onMount } from 'svelte';
	import { redirect } from '@sveltejs/kit';
	import { goto } from '$app/navigation';

	const { t } = getLocalization();
	let game_pin: string = '';

	navbarVisible.set(true);

	/*	interface StatsData {
		quiz_count: number;
		user_count: number;
	}*/

	/*	const getStats = async (): Promise<StatsData> => {
			const response = await fetch('/api/v1/stats/combined');
			return await response.json();
		};*/

	// eslint-disable-next-line no-unused-vars
	enum SelectedCreateThing {
		// eslint-disable-next-line no-unused-vars
		Create,
		// eslint-disable-next-line no-unused-vars
		Find,
		// eslint-disable-next-line no-unused-vars
		Import
	}

	// eslint-disable-next-line no-unused-vars
	enum SelectedPlayThing {
		// eslint-disable-next-line no-unused-vars
		Select,
		// eslint-disable-next-line no-unused-vars
		Results,
		// eslint-disable-next-line no-unused-vars
		Winners
	}

	let selected_create_thing = SelectedCreateThing.Create;
	let selected_play_thing = SelectedPlayThing.Select;

	/*	<li>No;
		Tracking < /li>
		< li > Self - hostable < /li>
		< li > German;
		Server < /li>
		< li > user - friendly < /li>
		< li > Completely;
		free < /li>
		< li > Quiz - results;
		are;
		downloadable < /li>;*/

	const classquiz_reasons = [
		{
			headline: $t('index_page.no_player_limit'),
			content: $t('index_page.no_player_limit_content')
		},
		{
			headline: $t('index_page.no_tracking'),
			content: $t('index_page.no_tracking_content')
		},
		{
			headline: $t('index_page.self_hostable'),
			content: $t('index_page.self_hostable_content')
		},
		{
			headline: $t('index_page.german_server'),
			content: $t('index_page.german_server_content')
		},
		{
			headline: $t('index_page.user_friendly'),
			content: $t('index_page.user_friendly_content')
		},
		{
			headline: $t('index_page.completely_free'),
			content: $t('index_page.completely_free_content')
		},
		{
			headline: $t('index_page.quiz_results_downloadable'),
			content: $t('index_page.quiz_results_downloadable_content')
		},
		{
			headline: $t('index_page.multilingual'),
			content: $t('index_page.multilingual_content')
		},
		{
			headline: $t('index_page.dark_mode'),
			content: $t('index_page.dark_mode_content')
		},
		{
			headline: $t('index_page.download_quizzes'),
			content: $t('index_page.download_quizzes_content')
		},
		{
			headline: $t('index_page.community_driven'),
			content: $t('index_page.community_driven_content')
		}
	];
	let selected_classquiz_reason = 0;

	const submitHandler = () =>{
		goto(`/play?pin=${game_pin}`);
	}
</script>

<svelte:head>
	<title>Zoni® AI - {$t('index_page.meta.title')}</title>
	<meta name="description" content={$t('index_page.meta.description')} />
	<title>Zoni® AI - Home</title>
	<meta
		name="description"
		content="At Zoni, our commitment to providing exceptional learning."
	/>

	<meta property="og:url" content="https://classquiz.de/" />
	<meta property="og:type" content="website" />
	<meta property="og:title" content="ClassQuiz - {$t('index_page.meta.title')}" />
	<meta
		property="og:description"
		content="At Zoni, our commitment to providing exceptional learning."
	/>
	<meta property="og:image" content={JpgOpenGraph} />

	<meta name="twitter:card" content="summary_large_image" />
	<meta property="twitter:domain" content="classquiz.de" />
	<meta property="twitter:url" content="https://classquiz.de/" />
	<meta name="twitter:title" content="Zoni AI - {$t('index_page.meta.title')}" />
	<meta
		name="twitter:description"
		content="At Zoni, our commitment to providing exceptional learning."
	/>
	<meta name="twitter:image" content={WebPOpenGraph} />
</svelte:head>
<!-- <section class="w-screen h-4/5 flex items-center justify-center" >
	 <div class="bg-[#7EF4EF] rounded-xl bg-opacity-50 w-1/4" >
		<div class="w-full justify-center flex" >
			<img src="{login_icon}" alt="" class="-mt-16">
		</div>
		<div class="flex flex-col items-center" >
			<p class="style-text font-bold mb-0" >Welcome to</p>
			<p class="text-white text-[3.4rem] -mt-7" >Zoni AI</p>
		</div>

		<div class="text-center" >
			<p class="text-white text-[14px]" >Log in or Create an account</p>
			<input type="text" class="rounded text-center bg-[#9FC7EB] text-[#75808A] p-2 shadow-inner" name="email" placeholder="Email or username" id="email">
		</div>
	 </div>
</section> -->
<section class="w-screen flex h-screen items-center justify-center" >
	<div class="bg-gradient-to-t from-[#D3E1EE] dark:from-[#00529B] to-[#FCFDFE] dark:to-[#00529B] shadow-[#003FA7]/50 shadow-md border border-[#003FA7]/50 rounded-xl dark:bg-gray-300 dark:shadow-none  bg-opacity-40 xl:w-1/4 sm:w-1/2  p-5 mt-20" >
		<div class="w-full justify-center flex" >
			<img src="{login_icon}" alt="" class="-mt-20">
		</div>
		<div class="flex flex-col items-center my-5" >
			<p class="text-[#003FA7] dark:text-white font-bold mb-0" >{$t('words.game_pin')}</p>
		</div>

		<form on:submit|preventDefault = {submitHandler} class="flex-col flex justify-center align-center mx-auto mb-12">
			<input
				class="border border-gray-400 md:w-1/2 w-full self-center text-center text-black bg-white bg-opacity-50 ring-0 shadow-inner  outline-none p-2 rounded-lg  transition-all"
				bind:value={game_pin}
				maxlength="6"
				inputmode="numeric"
			/>
			<!--				use:tippy={{content: "Please enter the game pin", sticky: true, placement: 'top'}}-->

			<br />
			<div class="mt-2 flex justify-center items-center">
				<button 
				class="px-5 py-2 flex items-center border-[#00EDFF] my-3 border-4 bg-gradient-to-r from-[#0056BD] dark:from-[#FFE500] from-0%  to-[#5436AB] dark:to-[#FFB800] to-100% leading-5 text-white transition-colors duration-200 transform rounded-full hover:bg-gray-600 focus:outline-none disabled:cursor-not-allowed disabled:opacity-50"
				 disabled={game_pin.length < 6} >
					<img src="{hand_click_icon}" alt="icon" class="" >
					{$t('words.submit')}
				</button>
				<!-- <BrownButton disabled={game_pin.length < 6}>{$t('words.submit')}</BrownButton> -->
			</div>
		</form>
	 </div>

</section>
<Footer />

<style>
	.style-text{
		-webkit-text-stroke: 1px white;
		color: transparent;
	
		text-shadow: 0px 0px 23px #0AECFE;
		font-size: 4rem;
		
	}
	.why-classquiz::-webkit-scrollbar {
		height: 0.8rem;
		margin-bottom: 5rem;
	}

	.why-classquiz::-webkit-scrollbar-track {
		box-shadow: inset 0 0 10px 10px transparent;
		border: solid 3px transparent;
	}

	.why-classquiz::-webkit-scrollbar-thumb {
		box-shadow: inset 0 0 10px 10px #374151;
		border: solid 3px transparent;
		border-radius: 15px;
	}

	.why-classquiz::-webkit-scrollbar-thumb:hover {
		box-shadow: inset 0 0 10px 10px #555;
		border: solid 3px transparent;
	}
</style>
