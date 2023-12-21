<script lang="ts">
  import Separator from "$lib/components/ui/separator/separator.svelte"

  type HeartRate = {
    heartRate: number
    timeStamp: number
  }

  const modes = [5_000, 10_000, 15_000]

  let heartRateList = $state<HeartRate[]>([])
  const currentHeartRate = $derived(heartRateList[heartRateList.length - 1])
  const averageHeartRates = $derived(modes.map((ms) => getAverageHeartRate(ms)))

  function getAverageHeartRate(ms: number) {
    const lastFiveSec = heartRateList.filter((item) => item.timeStamp > Date.now() - ms)
    const sum = lastFiveSec.reduce((acc, item) => acc + item.heartRate, 0)
    const avg = sum / lastFiveSec.length
    return avg.toFixed(1)
  }

  $effect(() => {
    const interval = setInterval(async () => {
      const res = await fetch("https://advent.sveltesociety.dev/data/2023/day-four.json")
      const data = await res.json()

      heartRateList.push({
        heartRate: data.heartRate,
        timeStamp: Date.now(),
      })
    }, 1000)

    return () => clearInterval(interval)
  })
</script>

<div>
  <p>Current Heart Rate: {currentHeartRate?.heartRate ?? "--"}</p>

  <Separator />

  <p>Averages</p>
  <ul>
    {#each averageHeartRates as avg, i (i)}
      <li>{modes[i] / 1000}s average: {averageHeartRates[i] || "--"}</li>
    {/each}
  </ul>
</div>
