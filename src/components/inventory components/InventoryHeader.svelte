<script lang="ts">
  import { applyAction, enhance } from '$app/forms';
  import { invalidateAll } from '$app/navigation';
  import { page } from '$app/stores';
  import { createToast, localisePrice } from '$lib';
  import { _ } from 'svelte-i18n';

  $: $page.form
    ? /* eslint-disable indent */
      createToast({
        header: $page.form.success ? $_('success') : $_('error'),
        message: $_($page.form?.messageKey),
        type: $page.form.success ? 'success' : 'error'
      })
    : null;
  /* eslint-disable indent */
</script>

<header class="container mt-6 mx-auto">
  <div class="flex justify-between">
    <a
      class="button justify-center h-10 px-5 font-medium rounded-lg w-36 md:w-48 bg-navy-800 text-navy-100 hover:text-white"
      href="/"
    >
      <svg class="icon w-2 h-2 flex-shrink-0 mr-2" viewBox="0 0 6.177 9.525" fill="none">
        <path
          d="M1985.484,56.559l4.055,4.055,4.055-4.055"
          transform="translate(62.029 -1984.777) rotate(90)"
          stroke="currentColor"
          stroke-width="2"
        ></path>
      </svg>
      <span class="text-2xs">{$_('profile.mainPage')}</span>
    </a>
    <form
      action="/login?/logout"
      method="POST"
      use:enhance="{() => {
        return async ({ result }) => {
          invalidateAll();
          await applyAction(result);
          window.location.reload();
        };
      }}"
    >
      <button
        type="submit"
        class="button items-center h-10 px-5 font-medium rounded-md w-36 md:w-48 bg-navy-800 text-2xs text-navy-100 hover:text-white"
      >
        <svg class="icon w-4 h-4 mr-2" viewBox="0 0 24 24" fill="currentColor">
          <path d="M18 6H20V18H18zM10 18L10 13 16 13 16 11 10 11 10 6 4 12z"></path>
        </svg>
        <span class="text-2xs">{$_('profile.logout')}</span>
      </button>
    </form>
  </div>
  <div class="flex flex-col items-center justify-between">
    <div class="flex flex-col items-center order-1 mx-6 mt-4 mb-2 lg:order-2 lg:w-1/3">
      <div class="mb-2 overflow-hidden border-2 border-solid w-36 h-36 border-gold rounded-3xl">
        <img
          src="https://cdn.cloudflare.steamstatic.com/steamcommunity/public/images/avatars/eb/ebd92149e5950221fcb87ca8475493b8e77833f3_full.jpg"
          class="object-contain w-full h-full"
          referrerpolicy="no-referrer"
          alt="pfp"
        />
      </div>
      <div class="flex items-center">
        <span
          class="inline-block max-w-xs overflow-hidden text-xl font-semibold leading-none text-center truncate"
        >
          {$page.data.user?.username}
        </span>
      </div>
      <!-- <a
        /// TODO add this
        class="mt-1 font-medium underline transition-colors duration-200 text-2xs text-navy-100 focus:outline-none focus-visible:ring-2 focus-visible:ring-gold hover:text-white css-1ug5voh"
        href="/pl/user/profile/76561199066122856"
      >
        PROFIL KEY-DROP
      </a> -->
    </div>
    <div class="order-2 w-full lg:order-3 lg:w-1/3">
      <div>
        <p
          class="flex items-center justify-center mb-1 text-xs font-semibold uppercase text-navy-100"
        >
          {$_('profile.balance')}
        </p>
        <div class="flex flex-col mb-4 md:flex-row">
          <div
            class="flex items-center px-4 mx-auto justify-center min-w-fit w-1/2 text-xs border border-solid rounded-lg bg-navy-700 border-navy-500 h-11"
          >
            <div class="flex items-center text-xs font-semibold text-white">
              <span class="text-gold">
                {localisePrice(page, $page.data.user.balance)}
                {$page.data.currency.toUpperCase()}
              </span>
            </div>
            <div class="flex items-center ml-4 text-xs font-semibold text-gold">
              <img src="/icons/gold-coin.webp" alt="" class="object-contain w-4 h-4 mr-1" />
              <span>{$page.data.user?.goldBalance}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</header>
