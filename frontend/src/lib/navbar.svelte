<!--
SPDX-FileCopyrightText: 2023 Marlon W (Mawoka)

SPDX-License-Identifier: MPL-2.0
-->

<script lang="ts">
    import '@fontsource/marck-script/index.css';
    import { getLocalization } from '$lib/i18n';
    import { signedIn, pathname } from '$lib/stores';
    import { page } from '$app/stores';
    import { createTippy } from 'svelte-tippy';
    import BrownButton from '$lib/components/buttons/brown.svelte';
    import { browser } from '$app/environment';
    import { beforeNavigate } from '$app/navigation';
    import { draw, slide } from 'svelte/transition';

    const tippy = createTippy({
        arrow: true,
        animation: 'perspective-subtle',
        placement: 'bottom'
    });
    const { t } = getLocalization();
    let menuIsClosed = true;
    const toggleMenu = () => {
        menuIsClosed = !menuIsClosed;
    };

    beforeNavigate(() => {
        menuIsClosed = true; // Closes menu to let the user see the page beneath
    });

    let darkMode = false;
    if (browser) {
        darkMode =
            localStorage.theme === 'dark' ||
            (!('theme' in localStorage) &&
                window.matchMedia('(prefers-color-scheme: dark)').matches);
    }

    const switchDarkMode = () => {
        darkMode = !darkMode;
        localStorage.setItem('theme', darkMode ? 'dark' : 'light');
        document.documentElement.classList.toggle('dark', darkMode);
    };
</script>

<nav class="w-screen px-4 lg:px-10 py-2 sticky  bg-[#F5FAFF] dark:bg-gray-200 shadow-md z-30 top-0">
    <!-- Desktop navbar -->
    <div class="hidden lg:flex lg:items-center lg:flex-row lg:justify-between">
        <div class="lg:flex lg:items-center lg:flex-row gap-1">
            <a href="/" class="link-hover px-3 lg:px-5">
                <img src="/zoni/images/zoni_logo.svg" alt="Zoni Logo" class="h-8 lg:h-10"> <!-- Adjust height as needed -->
            </a>
            <a class="btn-nav bg-[#0038FF] !px-9 !text-white rounded-lg" href="/play">{$t('words.play')}</a>
            <a class="btn-nav" class:active={$page.url.pathname === "/explore"} href="/explore">{$t('words.explore')}</a>
            <a class="btn-nav" class:active={$page.url.pathname === "/search"} href="/search">{$t('words.search')}</a>
            {#if $signedIn}
                <a class="btn-nav" class:active={$page.url.pathname === "/dashboard"} href="/dashboard">{$t('words.dashboard')}</a>
            {:else}
                <!--
                <a class="btn-nav" href="/docs">{$t('words.docs')}</a>
                <a
                    target="_blank"
                    class="btn-nav flex items-center gap-1"
                    href="https://github.com/mawoka-myblock/ClassQuiz"
                    >GitHub
                    <svg
                        xmlns="http://www.w3.org/2000/svg"
                        width="17"
                        height="17"
                        viewBox="0 0 24 24"
                        fill="none"
                        stroke="currentColor"
                        stroke-width="2"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        class="lucide lucide-external-link"
                        ><path
                            d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"
                        /><polyline points="15 3 21 3 21 9" /><line
                            x1="10"
                            x2="21"
                            y1="14"
                            y2="3"
                        /></svg
                    >
                </a>
                -->
            {/if}
        </div>
        <div class="lg:flex lg:items-center lg:flex-row gap-1">
            {#if $signedIn}
                <a class="btn-nav" href="/api/v1/users/logout">{$t('words.logout')}</a>
            {:else}
                {#if !import.meta.env.VITE_REGISTRATION_DISABLED}
                    <a class="btn-nav" href="/account/register">{$t('words.register')}</a>
                {/if}

                <a class="btn-nav" href="/account/login?returnTo={$pathname}">{$t('words.login')}</a
                >
            {/if}

            <div class="fit-content flex items-center justify-center gap-2">
                <!--
                <BrownButton href="https://mawoka.eu/donate" target="_blank"
                    >{$t('navbar.donate')} <span class="">❤️</span></BrownButton
                >
                -->
                <div class="lg:flex items-center justify-center">
                    {#if darkMode}
                        <button
                            on:click={switchDarkMode}
                            use:tippy={{ content: 'Switch light mode on' }}
                            aria-label="Activate light mode"
                        >
                            
                            <svg
                                class="w-6 h-6 text-black"
                                fill="none"
                                aria-label="Sun-Icon"
                                stroke="currentColor"
                                viewBox="0 0 24 24"
                                xmlns="http://www.w3.org/2000/svg"
                            >
                                <path
                                    stroke-linecap="round"
                                    stroke-linejoin="round"
                                    stroke-width="2"
                                    stroke="currentColor"
                                    d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z"
                                />
                            </svg>
                        </button>
                    {:else}
                        <button
                            on:click={switchDarkMode}
                            aria-label="Activate dark mode"
                            use:tippy={{ content: 'Switch dark mode on' }}
                        >
                            
                            <svg
                                aria-label="Moon-Icon"
                                class="w-6 h-6"
                                fill="none"
                                stroke="currentColor"
                                viewBox="0 0 24 24"
                                xmlns="http://www.w3.org/2000/svg"
                            >
                                <path
                                    stroke-linecap="round"
                                    stroke-linejoin="round"
                                    stroke-width="2"
                                    d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z"
                                />
                            </svg>
                        </button>
                    {/if}
                </div>
            </div>
        </div>
    </div>

    <!-- Mobile navbar -->
    <div class="lg:hidden">
        <!-- Navbar header -->
        <div class="flex items-center justify-between">
            <a href="/" class="link-hover px-3 lg:px-5">
                <img src="/zoni/images/zoni_logo.svg" alt="Zoni Logo" class="h-8 lg:h-10"> <!-- Adjust height as needed -->
            </a>
            <a class="btn-nav flex" href="/play">{$t('words.play')}</a>

            <!-- Dark/Light mode toggle + Open/Close menu -->
            <div class="flex items-center">
                {#if darkMode}
                    <!-- Sun icon -->
                    <button
                        class="px-3"
                        on:click={switchDarkMode}
                        use:tippy={{ content: 'Switch light mode on' }}
                        aria-label="Activate light mode"
                    >
                        <!-- Heroicons: sun -->
                        <svg
                            class="w-6 h-6 text-black"
                            fill="none"
                            aria-label="Sun-Icon"
                            stroke="currentColor"
                            viewBox="0 0 24 24"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                stroke-linecap="round"
                                stroke-linejoin="round"
                                stroke-width="2"
                                stroke="currentColor"
                                d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z"
                            />
                        </svg>
                    </button>
                {:else}
                    <!-- Moon icon -->
                    <button
                        class="px-3"
                        on:click={switchDarkMode}
                        aria-label="Activate dark mode"
                        use:tippy={{ content: 'Switch dark mode on' }}
                    >
                        <!-- Heroicons: moon -->
                        <svg
                            aria-label="Moon-Icon"
                            class="w-6 h-6"
                            fill="none"
                            stroke="currentColor"
                            viewBox="0 0 24 24"
                            xmlns="http://www.w3.org/2000/svg"
                        >
                            <path
                                stroke-linecap="round"
                                stroke-linejoin="round"
                                stroke-width="2"
                                d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z"
                            />
                        </svg>
                    </button>
                {/if}

                {#if menuIsClosed}
                    <button
                        class="px-3"
                        id="open-menu"
                        on:click={toggleMenu}
                        aria-label="Open navbar"
                    >
                        <svg
                            xmlns="http://www.w3.org/2000/svg"
                            width="24"
                            height="24"
                            viewBox="0 0 24 24"
                            fill="none"
                            stroke="#000000"
                            stroke-width="2"
                            stroke-linecap="round"
                            stroke-linejoin="round"
                        >
                            <path d="M3 6h18M3 12h18M3 18h18" />
                        </svg>
                    </button>
                {:else}
                    <button
                        class="px-3"
                        id="close-menu"
                        on:click={toggleMenu}
                        aria-label="Close navbar"
                    >
                        <svg
                            xmlns="http://www.w3.org/2000/svg"
                            width="24"
                            height="24"
                            viewBox="0 0 24 24"
                            fill="none"
                            stroke="#000000"
                            stroke-width="2"
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            ><path in:draw={{ duration: 300 }} d="M18 6 6 18" /><path
                                in:draw={{ duration: 300 }}
                                d="m6 6 12 12"
                            /></svg
                        >
                    </button>
                {/if}
            </div>
        </div>

        <!-- Navbar content -->
        {#if !menuIsClosed}
            <div class="flex flex-col" transition:slide={{ duration: 400 }}>
                <a class="btn-nav" class:active={$page.url.pathname === "/explore"} href="/explore">{$t('words.explore')}</a>
                <a class="btn-nav" class:active={$page.url.pathname === "/search"} href="/search">{$t('words.search')}</a>
                {#if $signedIn}
                    <a class="btn-nav" class:active={$page.url.pathname === "/dashboard"} href="/dashboard">{$t('words.dashboard')}</a>
                {:else}
                    <!--
                    <a class="btn-nav" href="/docs">{$t('words.docs')}</a>
                    <a
                        target="_blank"
                        class="btn-nav flex items-center gap-1"
                        href="https://github.com/mawoka-myblock/ClassQuiz"
                        >GitHub
                        <svg
                            xmlns="http://www.w3.org/2000/svg"
                            width="17"
                            height="17"
                            viewBox="0 0 24 24"
                            fill="none"
                            stroke="currentColor"
                            stroke-width="2"
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            class="lucide lucide-external-link"
                            ><path
                                d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"
                            /><polyline points="15 3 21 3 21 9" /><line
                                x1="10"
                                x2="21"
                                y1="14"
                                y2="3"
                            /></svg
                        >
                    </a>
                    -->
                {/if}

                <hr class="my-1 border" />
                {#if $signedIn}
                    <a class="btn-nav" href="/api/v1/users/logout">{$t('words.logout')}</a>
                {:else}
                    {#if !import.meta.env.VITE_REGISTRATION_DISABLED}
                        <a class="btn-nav" href="/account/register">{$t('words.register')}</a>
                    {/if}

                    <a class="btn-nav" href="/account/login?returnTo={$pathname}"
                        >{$t('words.login')}</a
                    >
                {/if}

                <!--
                <div class="fit-content flex items-center justify-center my-2">
                    <BrownButton href="https://mawoka.eu/donate" target="_blank"
                        >{$t('navbar.donate')} <span class="">❤️</span></BrownButton
                    >
                </div>
                -->
            </div>
        {/if}
    </div>
</nav>

<style lang="scss">
    .btn-nav {
        @apply text-lg font-medium px-3 text-gray-600 hover:text-[#00529B] py-1.5 transition-all duration-300;
    }
    .active {
        @apply text-[#00529B] font-bold;
    }
</style>
