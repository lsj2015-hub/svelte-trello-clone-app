<script>
  import Sortable from 'sortablejs'
  import { onMount } from 'svelte'
  import { lists } from '~/store/list.js'
  import List from '~/components/List.svelte'
  import CreateList from '~/components/CreateList.svelte'

  let listsEl
  let sortableLists

  onMount(() => {
    // For Lists
    sortableLists = new Sortable(listsEl, {
      group: 'My Lists',
      handle: '.list',
      delay: 50,
      animation: 0,
      forceFallback: true,
      onEnd(e) {
        console.log(e)
        lists.reorder({
          oldIndex: e.oldIndex,
          newIndex: e.newIndex
        })
      }
    })
  })
</script>

<div class="list-container">
  <div 
    bind:this={listsEl}
    class="lists">
    {#each $lists as list (list.id)}
      <List 
        {sortableLists} 
        {list} />
    {/each} 
  </div>
  <CreateList />
</div>

<style lang="scss">
  .list-container {
    width: 100vw;
    height: calc(100vh - 40px);
    padding: 30px;
    box-sizing: border-box;
    overflow-x: auto;
    overflow-y: hidden;
    white-space: nowrap;
    font-size: 0;
  }
  .lists {
    display: inline-block;
    height: 100%;
    white-space: nowrap;
    font-size: 0;
  }
</style>