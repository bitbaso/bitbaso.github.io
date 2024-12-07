<!-- src/lib/components/Navbar.svelte -->
<script>
    import { assets } from '$app/paths';

    let isDropdownOpen = false;
    let isMobileMenuOpen = false;
  
    const toggleDropdown = () => {
      isDropdownOpen = !isDropdownOpen;
    };
  
    const toggleMobileMenu = () => {
      isMobileMenuOpen = !isMobileMenuOpen;
    };
  </script>
  
  <nav class="bg-stone-900 py-4 text-white fixed w-full top-0 left-0 z-10">
    <div class="container mx-auto flex justify-between items-center px-4">
      <!-- Logo -->
      <a href="/" class="flex items-center hover:text-gray-300">
        <div class="rounded-lg bg-gradient-to-b from-green-600 to-slate-700 pr-2 pl-2 font-bold mr-2">b</div>
        <span class="text-lg font-bold">bitbaso</span>
      </a>
  
      <!-- Botón menú móvil -->
      <button class="md:hidden focus:outline-none" on:click={toggleMobileMenu}>
        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7" />
        </svg>
      </button>
  
      <!-- Menú -->
      <ul class="hidden md:flex items-center space-x-8">
        <!-- Dropdown Solutions -->
        <li class="relative">
          <button class="hover:text-gray-300 flex items-center focus:outline-none" on:click={toggleDropdown}>
            Solutions
            <svg class="w-4 h-4 ml-2" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7" />
            </svg>
          </button>
          {#if isDropdownOpen}
            <ul class="absolute top-full left-0 mt-2 w-48 bg-stone-800 text-white rounded shadow-lg">
              <li class="hover:bg-stone-700 px-4 py-2">
                <a href="https://github.com/bitbaso/Molde">Molde</a>
              </li>
              <li class="hover:bg-stone-700 px-4 py-2">
                <a href="https://github.com/bitbaso/Arindu">Arindu</a>
              </li>
              <li class="hover:bg-stone-700 px-4 py-2">
                <a href="https://github.com/bitbaso/MySqlCodeCreator">MySqlCodeCreator</a>
              </li>
            </ul>
          {/if}
        </li>       
        <li>
          <a href="https://github.com/bitbaso" class="hover:text-gray-300" target="_blank" title="github bitbaso">
            <img src={`${assets}/github-mark-white.png`} width="20px" alt="bitbaso github" />            
          </a>    
        </li>
      </ul>
    </div>
  
    <!-- Menú móvil -->
    {#if isMobileMenuOpen}
      <ul class="md:hidden bg-stone-900 px-4 py-2 space-y-2">
        <li>
          <button class="block w-full text-left hover:text-gray-300" on:click={toggleDropdown}>
            Solutions
          </button>
          {#if isDropdownOpen}
            <ul class="mt-2 bg-stone-800 rounded shadow-lg">
              <li class="hover:bg-stone-700 px-4 py-2">
                <a href="https://github.com/bitbaso/Molde">Molde</a>
              </li>
              <li class="hover:bg-stone-700 px-4 py-2">
                <a href="https://github.com/bitbaso/Arindu">Arindu</a>
              </li>
              <li class="hover:bg-stone-700 px-4 py-2">
                <a href="https://github.com/bitbaso/MySqlCodeCreator">MySqlCodeCreator</a>
              </li>
            </ul>
          {/if}
        </li>
      </ul>
    {/if}
  </nav>
  