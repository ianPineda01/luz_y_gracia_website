<script lang="ts">
  let { currentPage, onNavigate }: { currentPage: string; onNavigate: (page: string) => void } = $props();
  let menuOpen = $state(false);

  function toggleMenu() {
    menuOpen = !menuOpen;
  }
</script>

<nav class="nav">
  <a href="/" onclick={(e) => { e.preventDefault(); onNavigate('inicio'); }} class="brand">Luz y Gracia</a>

  <div class="links">
    {#each ['inicio', 'portafolio', 'contacto'] as page}
      <a
        href={page === 'inicio' ? '/' : `#${page}`}
        class="link"
        class:active={currentPage === page}
        onclick={(e) => { e.preventDefault(); onNavigate(page); }}
      >
        {page === 'inicio' ? 'Inicio' : page === 'portafolio' ? 'Portafolio' : 'Contacto'}
      </a>
    {/each}
  </div>

  <button class="hamburger" aria-label="Menú" onclick={toggleMenu}>
    <span class="bar"></span>
    <span class="bar"></span>
    <span class="bar"></span>
  </button>
</nav>

{#if menuOpen}
  <div class="mobile-menu">
    {#each ['inicio', 'portafolio', 'contacto'] as page}
      <a
        href={page === 'inicio' ? '/' : `#${page}`}
        class="mobile-link"
        class:active={currentPage === page}
        onclick={(e) => { e.preventDefault(); onNavigate(page); menuOpen = false; }}
      >
        {page === 'inicio' ? 'Inicio' : page === 'portafolio' ? 'Portafolio' : 'Contacto'}
      </a>
    {/each}
  </div>
{/if}

<style>
  .nav {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 1rem 2rem;
    border-bottom: 1px solid var(--border);
    background: var(--bg);
    position: sticky;
    top: 0;
    z-index: 100;
  }

  .brand {
    font-size: 1.35rem;
    font-weight: 700;
    color: var(--text-em);
    text-decoration: none;
    letter-spacing: 0.02em;
  }

  .links {
    display: flex;
    gap: 0.25rem;
  }

  .link {
    padding: 0.4rem 1rem;
    border-radius: 6px;
    color: var(--text);
    text-decoration: none;
    font-size: 0.95rem;
    transition: background 0.15s, color 0.15s;
  }

  .link:hover {
    background: var(--bg-subtle);
  }

  .link.active {
    color: var(--blue);
    background: var(--bg-subtle);
  }

  .hamburger {
    display: none;
    flex-direction: column;
    gap: 4px;
    background: none;
    border: none;
    cursor: pointer;
    padding: 4px;
  }

  .bar {
    display: block;
    width: 22px;
    height: 2px;
    background: var(--text-em);
    border-radius: 2px;
  }

  .mobile-menu {
    display: none;
    background: var(--bg);
    border-bottom: 1px solid var(--border);
    padding: 0.5rem 1rem;
  }

  .mobile-link {
    display: block;
    padding: 0.6rem 1rem;
    border-radius: 6px;
    color: var(--text);
    text-decoration: none;
  }

  .mobile-link.active {
    color: var(--blue);
    background: var(--bg-subtle);
  }

  @media (max-width: 640px) {
    .links { display: none; }
    .hamburger { display: flex; }
    .mobile-menu { display: block; }
  }
</style>
