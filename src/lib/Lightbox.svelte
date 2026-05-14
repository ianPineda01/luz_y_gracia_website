<script lang="ts">
  type Photo = { id: number; size: string; src: string; title?: string; color?: string; category?: string };

  let { photo, photos, onClose }: { photo: Photo; photos: Photo[]; onClose: () => void } = $props();
  let index = $derived(photos.indexOf(photo));

  function prev() {
    const i = (index - 1 + photos.length) % photos.length;
    photo = photos[i];
  }

  function next() {
    const i = (index + 1) % photos.length;
    photo = photos[i];
  }

  function onKeydown(e: KeyboardEvent) {
    if (e.key === 'Escape') onClose();
    if (e.key === 'ArrowLeft') prev();
    if (e.key === 'ArrowRight') next();
  }
</script>

<svelte:window onkeydown={onKeydown} />

<!-- svelte-ignore a11y_click_events_have_key_events a11y_no_interactive_element_to_noninteractive_role -->
<div class="backdrop" role="presentation" onclick={onClose}>
  <!-- svelte-ignore a11y_click_events_have_key_events -->
  <div class="lightbox" role="presentation" onclick={(e) => e.stopPropagation()}>
    {#if photo.src}
      <img src={import.meta.env.BASE_URL + photo.src} alt={photo.title} class="image" />
    {:else}
      <div class="placeholder-big" style="background: {photo.color}">
        <span>{photo.category} — Foto {photo.id}</span>
      </div>
    {/if}

    <button class="nav-btn prev" onclick={prev} aria-label="Anterior">‹</button>
    <button class="nav-btn next" onclick={next} aria-label="Siguiente">›</button>
    <button class="close-btn" onclick={onClose} aria-label="Cerrar">✕</button>

    {#if photo.title}
      <p class="caption">{photo.title}</p>
    {/if}
  </div>
</div>

<style>
  .backdrop {
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.85);
    z-index: 1000;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
  }

  .lightbox {
    position: relative;
    max-width: 90vw;
    max-height: 90vh;
    cursor: default;
  }

  .image {
    display: block;
    max-width: 90vw;
    max-height: 85vh;
    border-radius: 4px;
    object-fit: contain;
  }

  .placeholder-big {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 600px;
    max-width: 90vw;
    height: 400px;
    border-radius: 4px;
    color: #fdf6e3;
    font-size: 1.1rem;
    text-align: center;
  }

  .nav-btn {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    background: rgba(0,0,0,0.4);
    color: #fdf6e3;
    border: none;
    font-size: 2rem;
    width: 44px;
    height: 44px;
    border-radius: 50%;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: background 0.15s;
  }

  .nav-btn:hover {
    background: rgba(0,0,0,0.7);
  }

  .prev { left: -56px; }
  .next { right: -56px; }

  .close-btn {
    position: absolute;
    top: -44px;
    right: 0;
    background: none;
    color: #fdf6e3;
    border: none;
    font-size: 1.4rem;
    cursor: pointer;
    padding: 8px;
  }

  .caption {
    color: #eee8d5;
    text-align: center;
    margin: 0.5rem 0 0;
    font-size: 0.95rem;
  }

  @media (max-width: 640px) {
    .nav-btn {
      width: 36px;
      height: 36px;
      font-size: 1.4rem;
    }
    .prev { left: 4px; }
    .next { right: 4px; }
  }
</style>
