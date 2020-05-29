<script>
  import { onMount } from 'svelte'
  import { params, url } from '@sveltech/routify'
  const { source, category, name } = $params
  let item = null
  let cantidad = 100

  onMount(() => {
    item = fetch(`https://nutri.jenaro.dev/${source === 'longo' ? 'longo/' : ''}${category.split('-').map((p,i) => i === 1 ? p.toUpperCase() : p).join('')}/${name.replace(/%%/g, '%25%')}`).then(blob => blob.json())
  })

  const filterKeys = (arr) => arr.filter(key => !['field4', 'Nº', 'Alimento', 'Género - especie - variedad'].includes(key))

  const simpleThree = (cantidad, value) => {
    return Math.round((cantidad * value) / 100)
  }
</script>

<style>
  .table-container {
    display: flex;
    overflow-x: auto;
  }

  .title, .value {
    padding: 0.5rem 1rem;
    margin: 0;
    height: 4rem;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .title {
    justify-content: flex-start;
    background-color: var(--purple);
    color: var(--pink);
    border-bottom: 1px solid var(--pink);
  }

  .col {
    display: flex;
    flex: 1;
    flex-direction: column;
  }

  .value.key {
    text-align: center;
    background-color: #3C0E2F22;
  }

  .values .value {
    border-bottom: 1px solid var(--purple);
  }
</style>

<div class="ct">
<h1>{decodeURIComponent(name)}</h1>

<label for="cantidad">
  Cantidad (g/ml):
  <input name="cantidad" bind:value={cantidad} type="number"/>
</label>

{#if item}
  {#await item}
    <p>Cargando...</p>
  {:then values}
    {#if values.length > 1}
      <div class="table-container">
        <div class="col">
          {#each filterKeys(Object.keys(values[0])) as header}
            {#if typeof values[0][header] === "object"}
              {#each Object.keys(values[0][header]) as subKey}
                <p class="title">{header}</p>
              {/each}
            {/if}
            {#if typeof values[0][header] !== "object"}
              <p class="title">{header}</p>
            {/if}
          {/each}
        </div>
          {#each values as value}
            <div class="col values">
              {#each filterKeys(Object.keys(value)) as key}
                {#if typeof value[key] === "object"}
                  {#each Object.keys(value[key]) as subKey}
                    <p class={!isNaN(parseInt(value[key][subKey] || '0')) ? "value" : "value key"}>
                      {!isNaN(parseInt(value[key][subKey] || '0')) ? simpleThree(cantidad, value[key][subKey]) : `${subKey} (${value[key][subKey]})`}
                    </p>
                  {/each}
                {/if}
                {#if typeof value[key] !== "object"}
                  <p class={!isNaN(parseInt(value[key] || '0')) ? "value" : "value key"}>
                    {!isNaN(parseInt(value[key] || '0')) ? simpleThree(cantidad, value[key]) : value[key]}
                  </p>
                {/if}
              {/each}
            </div>
          {/each}
      </div>
    {/if}
  {/await}
{/if}
</div>