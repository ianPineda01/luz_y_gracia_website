<script lang="ts">
  import Nav from './lib/Nav.svelte';
  import Home from './lib/pages/Home.svelte';
  import Portfolio from './lib/pages/Portfolio.svelte';
  import Contact from './lib/pages/Contact.svelte';

  let currentPage = $state('inicio');

  function onNavigate(page: string) {
    currentPage = page;
    window.location.hash = page === 'inicio' ? '' : page;
  }

  // restore page from hash on load
  if (window.location.hash) {
    const hash = window.location.hash.slice(1);
    if (['portafolio', 'contacto'].includes(hash)) {
      currentPage = hash;
    }
  }

  window.addEventListener('hashchange', () => {
    const hash = window.location.hash.slice(1);
    if (['portafolio', 'contacto'].includes(hash)) {
      currentPage = hash;
    } else {
      currentPage = 'inicio';
    }
  });
</script>

<Nav {currentPage} {onNavigate} />

<main>
  {#if currentPage === 'inicio'}
    <Home />
  {:else if currentPage === 'portafolio'}
    <Portfolio />
  {:else if currentPage === 'contacto'}
    <Contact />
  {/if}
</main>

<footer class="footer">
  <p>© {new Date().getFullYear()} Luz y Gracia</p>
</footer>

<style>
  main {
    min-height: calc(100vh - 120px);
  }

  .footer {
    text-align: center;
    padding: 1.5rem 2rem;
    border-top: 1px solid var(--border);
    color: var(--text-muted);
    font-size: 0.85rem;
  }
</style>
