<script>
    import {page} from '$app/stores';
    import {connected, defaultEvmStores, selectedAccount} from "svelte-web3";

    export let background = "bg-transparent"
    export let color = "text-black dark:text-white"
    export let links = [
      {
        id: 0,
        href: "/",
        label: "Home"
      }
    ];

    let navbarOpen = false;

    function setNavbarOpen() {
        navbarOpen !== navbarOpen
    }

    

    function onClick() {
        if($connected) {
            return defaultEvmStores.disconnect()
        }
        else {
            return defaultEvmStores.setProvider()
        }
    }

</script>

<nav class="top-0 absolute z-50 w-full flex flex-wrap items-center justify-between px-2 py-3 navbar-expand-lg {background}">
    <div class="container px-4 mx-auto flex flex-wrap items-center justify-between">
        <div class="w-full relative flex justify-between lg:w-auto lg:static lg:block lg:justify-start">
            <a class="{color} text-sm font-bold leading-relaxed inline-block mr-4 py-2 whitespace-nowrap uppercase" href="/" sveltekit:prefetch>
                <slot name="brand">
                    Brand
                </slot>
            </a>
            <button class="cursor-pointer text-xl leading-none px-3 py-1 border border-solid border-transparent rounded bg-transparent block lg:hidden outline-none focus:outline-none"
                    on:click="{setNavbarOpen}"
                    type="button"
            >
                <i class="bg-white ni ni-air-baloon"></i>
            </button>
        </div>
        <div class="lg:flex flex-grow items-center {navbarOpen ? 'block':'hidden'}" id="navigation-navbar">
            <ul class="flex flex-col lg:flex-row list-none mr-auto">
                {#each links as link}
                    <li class:active={$page.url.pathname === link.href} class="flex items-center">
                        <a class="hover:text-blueGray-500 {color} px-3 py-2 flex items-center text-xs uppercase font-bold"
                        href="{link.href}" sveltekit:prefetch>
                            {link.label}
                        </a>
                    </li>
                {/each}
            </ul>
            <ul class="flex flex-col lg:flex-row list-none ml-auto">
                <li class="flex items-center">
                    <button class="border border-solid rounded bg-gradient-to-r from-cyan-500 to-blue-500 text-black px-3 py-1 block truncate" on:click={onClick} type="button">
                        {#if $connected}
                            {$selectedAccount}
                        {:else}
                            Connect
                        {/if}
                    </button>
                </li>
            </ul>
        </div>
    </div>
</nav>