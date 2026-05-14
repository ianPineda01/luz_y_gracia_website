<script lang="ts">
  import Lightbox from '../Lightbox.svelte';

  type Photo = { id: number; size: string; src: string; title?: string; color?: string; category?: string };

  let photos: Photo[] = [
    { id: 1,  size: 'large', src: 'portfolio/opt/B1_1.JPG' },
    { id: 2,  size: 'small', src: 'portfolio/opt/B1_2.JPG' },
    { id: 3,  size: 'wide', src: 'portfolio/opt/B1_3.JPG' },
    { id: 4,  size: 'tall',  src: 'portfolio/opt/B1_4.JPG' },
    { id: 5,  size: 'large',  src: 'portfolio/opt/B1_5.JPG' },
    { id: 6,  size: 'wide', src: 'portfolio/opt/B1_6.JPG' },
    { id: 7,  size: 'wide', src: 'portfolio/opt/B2_1.JPG' },
    { id: 8,  size: 'small', src: 'portfolio/opt/B2_2.JPG' },
    { id: 9,  size: 'tall',  src: 'portfolio/opt/B2_3.JPG' },
    { id: 10, size: 'tall',  src: 'portfolio/opt/B2_4.JPG' },
    { id: 11, size: 'wide', src: 'portfolio/opt/XV_1.JPG' },
    { id: 12, size: 'tall',  src: 'portfolio/opt/XV_2.JPG' },
    { id: 13, size: 'wide', src: 'portfolio/opt/XV_3.JPG' },
  ];

  let lightboxPhoto: Photo | null = $state(null);

  function openLightbox(photo: Photo) {
    lightboxPhoto = photo;
  }

  function closeLightbox() {
    lightboxPhoto = null;
  }
</script>

<section class="page">

  <div class="grid">
    {#each photos as photo (photo.id)}
      <button class="card {photo.size}" onclick={() => openLightbox(photo)} aria-label="Ver foto">
        {#if photo.src}
          <img src={import.meta.env.BASE_URL + photo.src} alt={photo.title} class="thumb" />
        {:else}
          <div class="placeholder" style="background: {photo.color}">
            <span class="placeholder-label">{photo.category} {photo.id}</span>
          </div>
        {/if}
      </button>
    {/each}
  </div>
</section>

{#if lightboxPhoto}
  <Lightbox
    photo={lightboxPhoto}
    photos={photos}
    onClose={closeLightbox}
  />
{/if}

<style>
  .page {
    padding: 3rem 2rem;
    max-width: 1100px;
    margin: 0 auto;
  }


  .grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-auto-rows: 200px;
    grid-auto-flow: dense;
    gap: 0.75rem;
  }

  .card {
    position: relative;
    border: none;
    border-radius: 8px;
    overflow: hidden;
    cursor: pointer;
    padding: 0;
    background: var(--bg-subtle);
    transition: transform 0.2s, box-shadow 0.2s;
  }

  .card:hover {
    transform: translateY(-3px);
    box-shadow: 0 6px 20px rgba(0,0,0,0.12);
  }

  .small  { grid-column: span 1; grid-row: span 1; }
  .wide   { grid-column: span 2; grid-row: span 1; }
  .tall   { grid-column: span 1; grid-row: span 2; }
  .large  { grid-column: span 2; grid-row: span 2; }

  @media (max-width: 700px) {
    .grid { grid-template-columns: repeat(2, 1fr); grid-auto-rows: 180px; }
    .tall  { grid-column: span 1; grid-row: span 1; }
    .wide  { grid-column: span 2; grid-row: span 1; }
    .large { grid-column: span 2; grid-row: span 1; }
  }

  .thumb {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
  }

  .placeholder {
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .placeholder-label {
    color: #fdf6e3;
    font-size: 0.85rem;
    opacity: 0.8;
  }
</style>
