<script>
  import { writable } from 'svelte/store';
  import Transition from "../components/Transition.svelte"
  const width = writable();
</script>

<div bind:offsetWidth={$width} style="height: 100%;">
  <slot decorator={Transition} scoped={{ width }} />
</div>

