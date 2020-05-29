<script>
  import { url } from '@sveltech/routify'
  import { writable } from 'svelte/store';
  import Transition from "../../../../components/Transition.svelte"
  const width = writable();
</script>

<div class="bar"><a href={$url("../")}>&larr; Atras</a></div>
<div bind:offsetWidth={$width}>
  <slot decorator={Transition} scoped={{ width }} />
</div>
