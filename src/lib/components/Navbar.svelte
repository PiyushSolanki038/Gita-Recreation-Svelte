<script>
  import { onMount } from "svelte";

  let openMenu = null;
  let navEl;

  const dropdowns = {
    ayurveda: ["Introduction", "Timing", "Food"],
    yogasutras: ["Introduction", "Obstacles", "Practices", "Yogāsana", "Glossary"],
    upanisads: [
      "Īsopaniṣad",
      "Taittirīyopaniṣad",
      "Māndūkyopaniṣad",
      "Kenopaniṣad",
      "Kaṭhopaniṣad",
      "Aitareyopaniṣad",
      "Muṇḍakopaniṣad",
      "Praśnopaniṣad"
    ],
    sanskrit: ["Devotional Songs", "Grammar Songs", "Fun Songs", "Script Songs"]
  };

  const toggleMenu = (name, e) => {
    e.stopPropagation();
    openMenu = openMenu === name ? null : name;
  };

  const handleDocClick = (e) => {
    if (!navEl.contains(e.target)) openMenu = null;
  };

  onMount(() => {
    document.addEventListener("click", handleDocClick);
    return () => document.removeEventListener("click", handleDocClick);
  });
</script>

<nav class="navbar" bind:this={navEl}>
  <div class="nav-left">
    <div class="logo">
      <img src="/logo.png" alt="logo" />
    </div>
  </div>

  <div class="nav-center"></div>

  <div class="nav-right">
    <ul class="menu" role="menubar">
      
      <!-- SEARCH ICON -->
      <li class="search-btn item-relative" role="menuitem" title="Search" tabindex="0">
        <div class="item-inner">
          <svg
            width="20" height="20" viewBox="0 0 24 24" fill="none"
            stroke="black" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"
          >
            <circle cx="11" cy="11" r="7"></circle>
            <line x1="16.5" y1="16.5" x2="22" y2="22"></line>
          </svg>
          <span class="underline search-underline"></span>
        </div>
      </li>

      <!-- AYURVEDA -->
      <li
        class="menu-item item-relative"
        role="menuitem"
        on:click|stopPropagation={(e) => toggleMenu('ayurveda', e)}
        on:mouseenter={() => (openMenu = 'ayurveda')}
        on:mouseleave={() => (openMenu = null)}
        class:open={openMenu === 'ayurveda'}
      >
        <div class="item-inner">
          <div class="item-row">
            <span class="item-text">ĀYURVEDA</span>
            <svg class="caret" width="10" height="10" viewBox="0 0 24 24" fill="none"
              stroke="black" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
              <polyline points="6 9 12 15 18 9"></polyline>
            </svg>
          </div>
          <span class="underline"></span>
        </div>

        {#if openMenu === 'ayurveda'}
          <div class="dropdown"
            on:mouseenter={() => (openMenu = 'ayurveda')}
            on:mouseleave={() => (openMenu = null)}
            on:click|stopPropagation
          >
            {#each dropdowns.ayurveda as item}
              <a href="#" class="dd-link">{item}</a>
            {/each}
          </div>
        {/if}
      </li>

      <!-- YOGASUTRAS -->
      <li
        class="menu-item item-relative"
        role="menuitem"
        on:click|stopPropagation={(e) => toggleMenu('yogasūtras', e)}
        on:mouseenter={() => (openMenu = 'yogasūtras')}
        on:mouseleave={() => (openMenu = null)}
        class:open={openMenu === 'yogasūtras'}
      >
        <div class="item-inner">
          <div class="item-row">
            <span class="item-text">YOGASŪTRAS</span>
            <svg class="caret" width="10" height="10" viewBox="0 0 24 24" fill="none"
              stroke="black" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
              <polyline points="6 9 12 15 18 9"></polyline>
            </svg>
          </div>
          <span class="underline"></span>
        </div>

        {#if openMenu === 'yogasūtras'}
          <div class="dropdown"
            on:mouseenter={() => (openMenu = 'yogasūtras')}
            on:mouseleave={() => (openMenu = null)}
            on:click|stopPropagation
          >
            {#each dropdowns.yogasutras as item}
              <a href="#" class="dd-link">{item}</a>
            {/each}
          </div>
        {/if}
      </li>

      <!-- BHAGAVAD GITA -->
      <li class="menu-item item-relative" role="menuitem">
        <div class="item-inner">
          <div class="item-row">
            <span class="item-text">BHAGAVAD GĪTĀ</span>
          </div>
          <span class="underline"></span>
        </div>
      </li>

      <!-- UPANISADS -->
      <li
        class="menu-item item-relative"
        role="menuitem"
        on:click|stopPropagation={(e) => toggleMenu('upanisads', e)}
        on:mouseenter={() => (openMenu = 'upanisads')}
        on:mouseleave={() => (openMenu = null)}
        class:open={openMenu === 'upanisads'}
      >
        <div class="item-inner">
          <div class="item-row">
            <span class="item-text">UPANIṢADS</span>
            <svg class="caret" width="10" height="10" viewBox="0 0 24 24" fill="none"
              stroke="black" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
              <polyline points="6 9 12 15 18 9"></polyline>
            </svg>
          </div>
          <span class="underline"></span>
        </div>

        {#if openMenu === 'upanisads'}
          <div class="dropdown"
            on:mouseenter={() => (openMenu = 'upanisads')}
            on:mouseleave={() => (openMenu = null)}
            on:click|stopPropagation
          >
            {#each dropdowns.upanisads as item}
              <a href="#" class="dd-link">{item}</a>
            {/each}
          </div>
        {/if}
      </li>

      <!-- SANSKRIT -->
      <li
        class="menu-item item-relative"
        role="menuitem"
        on:click|stopPropagation={(e) => toggleMenu('sanskrit', e)}
        on:mouseenter={() => (openMenu = 'sanskrit')}
        on:mouseleave={() => (openMenu = null)}
        class:open={openMenu === 'sanskrit'}
      >
        <div class="item-inner">
          <div class="item-row">
            <span class="item-text">SANSKRIT</span>
            <svg class="caret" width="10" height="10" viewBox="0 0 24 24" fill="none"
              stroke="black" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">
              <polyline points="6 9 12 15 18 9"></polyline>
            </svg>
          </div>
          <span class="underline"></span>
        </div>

        {#if openMenu === 'sanskrit'}
          <div class="dropdown"
            on:mouseenter={() => (openMenu = 'sanskrit')}
            on:mouseleave={() => (openMenu = null)}
            on:click|stopPropagation
          >
            {#each dropdowns.sanskrit as item}
              <a href="#" class="dd-link">{item}</a>
            {/each}
          </div>
        {/if}
      </li>

      <!-- CONTACT -->
      <li class="menu-item item-relative" role="menuitem">
        <div class="item-inner">
          <div class="item-row">
            <span class="item-text">CONTACT US</span>
          </div>
          <span class="underline"></span>
        </div>
      </li>

    </ul>
  </div>
</nav>

<style>
  :global(body) { margin: 0; }

  nav.navbar {
    width: 100%;
    display: flex;
    align-items: center;
    margin-top: 19px;
    padding: 16px 28px;
    box-sizing: border-box;
    font-family: 'Inter', system-ui, -apple-system, "Segoe UI", Roboto, Arial;
    position: relative;
    z-index: 10;
    margin-bottom: 10px;
  }

  .nav-left { flex: 0 0 auto; display:flex; align-items:center; }
  .nav-center { flex: 1; }
  .nav-right { flex: 0 0 auto; display:flex; align-items:center; }

  .logo img { height: 56px; display:block; }

  .menu {
    display: flex;
    gap: 10px;
    list-style: none;
    margin: 0;
    padding: 0;
    align-items: center;
    white-space: nowrap;
  }

  .menu-item, .search-btn {
    cursor: pointer;
    font-size: 12px;
    font-weight: 400;
    color: #111;
    display: inline-flex;
    align-items: center;
    gap: 6px;
    padding: 4px 2px;
    position: relative;
  }

  .menu-item:hover .item-text,
  .menu-item:hover .caret,
  .search-btn:hover svg {
    color: #D9CBA7;
    stroke: #D9CBA7;
  }

  .search-underline { width: 20px !important; }

  .item-inner { display: flex; flex-direction: column; align-items:center; padding:2px 6px; }

  .item-row { display:flex; gap:6px; }

  .caret {
    margin-top: 2px;
    opacity: 0.75;
    transition: transform 0.2s ease;
  }

  .open .caret { transform: rotate(180deg); }

  .underline {
    height: 2px;
    width: 90px;
    background: black;
    margin-top: 10px;
    transform: scaleX(0);
    transform-origin: left;
    transition: 0.45s ease;
  }

  .menu-item:hover .underline,
  .search-btn:hover .underline {
    transform: scaleX(1);
  }

  .dropdown {
    position: absolute;
    top: calc(100% + 10px);
    left: 50%;
    transform: translateX(-50%);
    min-width: 160px;
    background: white;
    box-shadow: 0 10px 22px rgba(15,15,15,0.12);
    border-radius: 8px;
    padding: 8px 0;
    z-index: 999;
    display: flex;
    flex-direction: column;
  }

  .dd-link {
    padding: 8px 14px;
    font-size: 14px;
    text-decoration: none;
    color: #111;
  }

  .dd-link:hover { background: rgba(0,0,0,0.04); }

  @media (max-width: 1000px) {
    .menu { gap: 18px; }
    .underline { width: 30px !important; }
    .logo img { height: 50px; }
  }

  @media (max-width: 700px) {
    nav.navbar { padding: 10px 12px; }
    .menu { gap: 12px; font-size: 13px; }
  }

  /* ==============================
   NAVBAR – RESPONSIVE DESIGN
============================== */

@media (max-width: 900px) {
  nav.navbar {
    padding: 12px 18px;
  }

  .logo img {
    height: 48px;
  }

  .menu {
    gap: 14px;
  }

  .item-text {
    font-size: 11px;
  }
}

@media (max-width: 700px) {
  nav.navbar {
    padding: 10px 12px;
  }

  .menu {
    gap: 10px;
  }

  .item-text {
    font-size: 10px;
  }

  .caret {
    width: 8px;
    height: 8px;
  }

  .underline {
    width: 40px;
  }
}

@media (max-width: 480px) {
  .menu {
    gap: 6px;
  }

  .item-text {
    font-size: 9px;
  }

  .logo img {
    height: 40px;
  }
}

</style>
