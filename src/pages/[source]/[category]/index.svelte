<script>
  import { onMount  } from 'svelte'
  import { params, url, goto } from '@sveltech/routify'
  let items = null
  const { source, category } = $params

  const spanishName = {
    egg: 'Huevo',
    vegetable: 'Vegetal',
    cereal: 'Cereal',
    fat: 'Grasa',
    fish: 'Pescado',
    'fish-ag': 'Pescado AG',
    meat: 'Carne',
    'meat-ag': 'Carne AG',
    fruit: 'Fruta',
    milk: 'Leche',
    misc: 'Misc',
    'prod-az': 'Prod AZ'
  }

  onMount(() => {
    if (!spanishName[category]) return $goto('/')
    items = fetch(`https://nutri.jenaro.dev/${source === 'longo' ? 'longo/' : ''}${category.split('-').map((p,i) => i === 1 ? p.toUpperCase() : p).join('')}`).then(blob => blob.json())
  })

  const fetchItems = async (e) => {
    if (!e.target.name.value) return
    items = fetch(`https://nutri.jenaro.dev/${source === 'longo' ? 'longo/' : ''}${category.split('-').map((p,i) => i === 1 ? p.toUpperCase() : p).join('')}/${e.target.name.value}`).then(blob => blob.json())
  }
</script>

<style>
  h1 {
    text-transform: capitalize;
  }

  a {
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: calc(100% - 40px);
    padding: 10px 20px;
    margin-bottom: 1rem;
    background: var(--purple);
    color: var(--pink);
  }
</style>

<div class="ct">
  <h1>{spanishName[category]}</h1>

  <form on:submit|preventDefault={fetchItems}>
    <label id="name">
      Nombre:
      <input id="name" name="name" />
    </label>
    <button>Buscar</button>
  </form>

  {#if items}
    {#await items}
      <p>Cargando...</p>
    {:then values}
      {#each values as value}
        {#if value.Alimento}
          <a href={$url(`/${source}/${category}/${encodeURIComponent(value.Alimento)}`)}>{value.Alimento} <span>&rarr;</span></a>
        {/if}
      {/each}
    {/await}
  {/if}
</div>