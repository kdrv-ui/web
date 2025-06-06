<script lang="ts">
  // Define props
  let { activePage } = $props();

  import { onMount, onDestroy } from 'svelte';
  import { writable } from 'svelte/store';

  // Navigation items
  const navItems = [
    { href: '/', label: 'Home', key: 'home' },
    { href: '/docs', label: 'Documentation', key: 'docs' },
    { href: '/about', label: 'About', key: 'about' },
    { href: '/contact', label: 'Contact', key: 'contact' },
    { href: '/components', label: 'Components', key: 'components' }
  ];

  // Store to track if the page is scrolled
  const isScrolled = writable(false);

  function handleScroll() {
    isScrolled.set(window.scrollY > 10);
  }

  onMount(() => {
    window.addEventListener('scroll', handleScroll);
    handleScroll();
    return () => window.removeEventListener('scroll', handleScroll);
  });
</script>

<header class="p-4 md:p-6 flex flex-col md:flex-row justify-between md:items-center sticky top-0 z-50 transition-all duration-300" class:backdrop-blur-sm={$isScrolled}>
  <div class="flex items-center gap-2 justify-center md:justify-start w-full md:w-auto">
    <div class="inline-flex items-center justify-center text-white" style="width: 8rem; height: 3rem;">
      {#if $isScrolled}
        <img 
          src="/images/logo/main.svg" 
          alt="Logo" 
          class="h-14 w-14 mx-auto animate-fade-in rounded-sm" 
          style="animation: fadeIn 0.3s ease-in-out;"
        />
      {:else}
        <div 
          class="text-xl font-bold leading-none m-auto animate-slide-in bg-black rounded-sm p-2 "
          style="animation: slideIn 0.3s ease-in-out;"
        >
          KDRV
        </div>
      {/if}
      </div>

    <style>
      @keyframes fadeIn {
        from {
          opacity: 0;
          transform: scale(0.8);
        }
        to {
          opacity: 1;
          transform: scale(1);
        }
      }

      @keyframes slideIn {
        from {
          opacity: 0;
          transform: translateY(-10px);
        }
        to {
          opacity: 1;
          transform: translateY(0);
        }
      }

      .bg-blur {
        @apply backdrop-blur-md bg-white/30;
      }
    </style>
  </div>
  
  <nav class="hidden md:flex space-x-1 justify-center w-full ">
    {#each navItems as item}
      <a 
        href={item.href}
        class={`font-medium  px-4 py-2 transition-all duration-300 ease-in-out hover:shadow-lg  first:rounded-l-4xl last:rounded-r-4xl ${activePage === item.key ? 'text-bianca rounded-4xl bg-black' : 'text-primary hover:text-bianca hover:bg-black hover:rounded-4xl rounded-xs bg-highlight'}`}
      >
        {item.label}
      </a>
    {/each}
  </nav>
  
  <div class="md:hidden flex justify-end w-full mt-2 md:mt-0">
    <button class="text-primary p-2">
      <!-- Simple hamburger menu icon -->
      <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
      </svg>
    </button>
  </div>
</header>