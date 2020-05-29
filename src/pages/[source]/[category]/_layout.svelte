<script>
  import { url } from '@sveltech/routify'
  import { writable } from 'svelte/store';
  import Transition from "../../../components/Transition.svelte"
  const width = writable();
</script>

<style>
  a {
    display: block;
    width: 100%;
  }
</style>

<div class="bar"><a href={$url("../")}>&larr; Atras</a></div>
<div bind:offsetWidth={$width} style="height: 100%;">
  <slot decorator={Transition} scoped={{ width }} />
</div>
