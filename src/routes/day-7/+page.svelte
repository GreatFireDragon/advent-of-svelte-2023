<script lang="ts">
  import { Input } from "$lib/components/ui/input"
  import { Button } from "$lib/components/ui/button"
  import { morseMap } from "./morseMap"

  let inputVal = $state("")
  let morseCode: string[] = $derived(convertToMorse())

  function convertToMorse() {
    const inputArr = inputVal.toUpperCase().split("")
    const morseArr: string[] = []

    inputArr.forEach((item) => {
      const morseChar: string | undefined = morseMap.get(item)
      if (morseChar) morseArr.push(morseChar)
    })

    return morseArr
  }
</script>

<Input bind:value={inputVal} />

{#if morseCode.length}
  {@const code = morseCode.join("")}
  <p>{code}</p>
{/if}
