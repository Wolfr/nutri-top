<script>
  import { url } from '@sveltech/routify'
  import { writable } from 'svelte/store';
  import Transition from "../../components/Transition.svelte"
  const width = writable();
</script>

<main bind:offsetWidth={$width} style="height: 100%;">
  <slot decorator={Transition} scoped={{ width }} />
</main>
