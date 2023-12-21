<script>
  import AdventsNav from "$lib/components/AdventsNav.svelte"
  import "../app.pcss"
</script>

<header class="flex flex-col items-center px-10">
  <h1>Advent of Svelte 2023</h1>
  <p>Svelte 5 + SvelteKit 2 + TailwindCSS</p>
  <AdventsNav />
</header>

<main class="p-4">
  <slot />
</main>
