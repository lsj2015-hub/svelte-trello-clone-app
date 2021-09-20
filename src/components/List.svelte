<script>
  import Sortable from 'sortablejs'
  import { onMount } from 'svelte'
  import { cards } from '~/store/list'
  import CreateCard from '~/components/CreateCard.svelte'
  import ListTitle from '~/components/ListTitle.svelte'
  import Card from '~/components/Card.svelte'

  export let sortableLists
  export let list 

  let cardsEl
  let sortableCards

  function disableSortable(e) {
    sortableLists.option('disabled', e.detail)
    sortableCards.option('disabled', e.detail)
  }

  onMount(() => {
    // For Lists
    sortableCards = new Sortable(cardsEl, {
      group: 'My cards',
      handle: '.card',
      delay: 50,
      forceFallback: true,
      onEnd(e) {
        console.log(e)
        cards.reorder({
          fromListId: e.from.dataset.listId,
          toListId: e.to.dataset.listId,
          oldIndex: e.oldIndex,
          newIndex: e.newIndex
        })
      }
    })
  })
</script>

<div class="list">
  <div class="list__inner">
    <div class="list__heading">
      <ListTitle 
        {list} 
        on:editMode={disableSortable} />
      <p>{list.cards.length} cards</p>
    </div>
    <div 
      data-list-id={list.id}
      bind:this={cardsEl}
      class="list__cards">
      {#each list.cards as card (card.id)}
        <Card 
          listId={list.id} 
          {card} 
          on:editMode={disableSortable} />
      {/each}
    </div>
    <CreateCard 
      listId={list.id}
      on:editMode={disableSortable} />
  </div>
</div>

<style lang="scss">
  .list {
    display: inline-block;
    font-size: 16px;
    white-space: normal;
    width: 290px;
    height: 100%;
    box-sizing: border-box;
    margin: 0 4px;
    line-height: 20px;
    :global(&.sortable-ghost) {
      opacity: .2;
      position: relative;
      &::after {
        content: "";
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: #000;
        border-radius: 4px;
      }
    }
    :global(&.sortable-chosen) {
      cursor: move;
    }
    .list__inner {
      display: flex;
      flex-direction: column;
      max-height: 100%;
      padding: 10px 8px;
      box-sizing: border-box;
      background: #ebecf0;
      border-radius: 4px;
    }
    .list__heading {
      margin-bottom: 10px;
      p {
        color: #5e6c84;
        padding: 0 8px;
      }
    }
    .list__cards {
      overflow-x: hidden;
      overflow-y: auto;
      margin-bottom: 10px;
    }
  }
</style>