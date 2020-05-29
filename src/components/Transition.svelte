<script>
  import { BaseTransition } from "@sveltech/routify/decorators"
  import { cubicInOut } from 'svelte/easing';
  
  export function flyCustom(node, {
      delay = 0,
      duration = 400,
      easing = cubicOut,
      x = 0,
      y = 0
  }) {
      const style = getComputedStyle(node);
      const transform = style.transform === 'none' ? '' : style.transform;
      return {
          delay,
          duration,
          easing,
          css: (t, u) => `transform: ${transform} translate(${(1 - t) * x}px, ${(1 - t) * y}px);`
      };
  }

  export let scoped
  const { width } = scoped
  const configs = [
    {
      condition: ({ routes }) => routes[0] === routes[1],
      transition: () => {},
    },
    {
      condition: c => c.toDescendant,
      transition: flyCustom,
      inParams: { x: $width, duration: 600, easing: cubicInOut },
      outParams: { x: -$width, duration: 600, easing: cubicInOut },
    },
    {
      condition: c => c.toAncestor,
      transition: flyCustom,
      inParams: { x: -$width, duration: 600, easing: cubicInOut },
      outParams: { x: $width, duration: 600, easing: cubicInOut },
    },
    {
      // No matching config. We don't want a transition
      condition: () => true,
      transition: () => {},
    },
  ]
</script>

<BaseTransition {configs}>
  <slot />
</BaseTransition>
