<script lang="ts">
  import ProfilePicture from './profile-picture.svelte';
  import { page } from '$app/stores';

  import showHamburgerMenu from '$lib/stores/show-hamburger-menu';
  import HamburgerMenu from './hamburger-menu/menu.svelte';
  import { onMount } from 'svelte';
  import { showHideOverflowY } from '../util/hide-overflow';

  onMount(() => {
    const handleTabletChange = (e: MediaQueryListEvent) => {
      console.log('changed');
      if (e.matches) {
        $showHamburgerMenu = false;
        showHideOverflowY(false);
      }
    };
    let query = window.matchMedia('(min-width: 768px)');
    query.addEventListener('change', handleTabletChange);
  });

  export let username: string | undefined;

  const navItems = [{ name: 'Docs', path: '/docs' }];
</script>

<header class="my-8 flex flex-wrap items-center justify-between">
  <a href="/">
    <img src="/images/hubber.png" class="w-8 object-contain md:w-12" alt="hubber" />
  </a>
  <div class="lg:hidden">
    <button
      on:click={() => ($showHamburgerMenu = true)}
      class="navbar-burger flex items-center p-3 text-neutral-900"
      data-test-id="hamburger-btn"
    >
      <svg
        class="block h-4 w-4 fill-skin-text-highlight"
        viewBox="0 0 20 20"
        xmlns="http://www.w3.org/2000/svg"
      >
        <title>Mobile menu</title>
        <path d="M0 3h20v2H0V3zm0 6h20v2H0V9zm0 6h20v2H0v-2z" />
      </svg>
    </button>
  </div>
  <div class="hidden items-center gap-4 lg:flex lg:gap-8">
    <ul>
      {#each navItems as item}
        <li>
          <a
            class="default-transition text-skin-text-highlight decoration-transparent hover:underline hover:decoration-inherit"
            class:active={$page.url.pathname.includes(item.path)}
            href={item.path}>{item.name}</a
          >
        </li>
      {/each}
    </ul>
    {#if username}
      <ProfilePicture {username} />
    {:else}
      <a
        data-test-id="login-btn-lg"
        class="default-transition rounded-xl bg-skin-text px-4 py-2 text-skin-off-background hover:bg-primary-100 dark:bg-skin-text-highlight dark:text-skin-background dark:hover:bg-primary-100"
        href="/login">Login</a
      >
    {/if}
  </div>

  <HamburgerMenu {navItems} {username} />
</header>

<style lang="postcss">
  .active {
    @apply underline decoration-inherit;
  }
</style>
