<script>
  import { Canvas } from '@threlte/core'
  import Scene from './Scene.svelte'
  import { Pane, Slider, List } from 'svelte-tweakpane-ui'

  let minSeconds = 5
  $: maxSeconds = 30
</script>

<!-- <Pane
  title="Innstillinger"
  expanded={false}
>
  <Slider
    label="Maks antall sekunder"
    value={maxSeconds}
    min={minSeconds}
    max={60}
    step={1}
  />
</Pane> -->

<Canvas>
  <Scene {maxSeconds}/>
</Canvas>