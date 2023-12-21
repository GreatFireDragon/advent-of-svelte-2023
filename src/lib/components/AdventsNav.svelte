<script>
  import Button from "$lib/components/ui/button/button.svelte"

  const days = Array.from({ length: 24 }, (_, i) => i + 1)
</script>

<nav class="flex w-full gap-2 overflow-x-scroll">
  {#each days as day}
    <Button variant="outline">
      <a href="/day-{day}">DAY {day}</a>
    </Button>
  {/each}
</nav>
