<script lang="ts">
  import type { Present } from "./+page.js"
  import Button from "$lib/components/ui/button/button.svelte"
  import Progress from "$lib/components/ui/progress/progress.svelte"

  let { data } = $props()

  let backlogPresents = $state([...data.presents])
  let sleighPresents = $state<Present[]>([])

  const MAX_LOAD = 100
  const currentLoad = $derived(
    Number(sleighPresents.reduce((acc, item) => acc + item.weight, 0).toFixed(2))
  )

  const isOverloaded = $derived(currentLoad > MAX_LOAD)

  $effect(backlogPresents.sort((a, b) => a.weight - b.weight))
</script>

<div>
  Currend load: {currentLoad}
  <Progress value={currentLoad} max={MAX_LOAD} />
</div>

<div class="flex items-start w-full gap-4">
  <div class="grid w-full grid-cols-3 border rounded-md">
    {#each backlogPresents as present, i (`${present.name}-${present.weight}`)}
      <Button
        variant="outline"
        on:click={() => sleighPresents.push(backlogPresents.splice(i, 1)[0])}
      >
        {present.name} — {present.weight}
      </Button>
    {/each}
  </div>

  <div class="grid w-full grid-cols-3 border rounded-md">
    {#each sleighPresents as present, i (`${present.name}-${present.weight}`)}
      <Button
        variant="outline"
        class="hover:bg-red-200"
        on:click={() => {
          backlogPresents.push(sleighPresents.splice(i, 1)[0])
        }}
      >
        {present.name} — {present.weight}
      </Button>
    {/each}
  </div>
</div>
