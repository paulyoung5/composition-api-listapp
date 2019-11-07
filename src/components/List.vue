<template>
    <div class="list">
        <h1>Shopping list 🛒</h1>

        <transition-group
            tag="ul"
            name="items"
            :class=" { 'finished': (incompleteCount === 0) } "
        >
            <li
                v-for="{ id, name, done } in sortedItems"
                :key="id"
                :class="{ 'done': done }"
            >
                <span>
                    {{ name }}
                </span>
                
                <button class="mark-done" type="button" @click="markDone(id)">
                    Mark done
                </button>
            </li>
            
            <div class="complete-message" key="message">
                <h2>✅</h2>

                <h2>All done</h2>
                <p>Head to the checkout!</p>
                <button class="reset" type="button" @click="reset">
                    Or start again..
                </button>
            </div>
        </transition-group>
    </div>
</template>

<script lang="ts">
import {
  createComponent,
  computed,
  reactive,
  watch,
  toRefs
} from '@vue/composition-api'
import items from '../items'
import Item from './../types/Item'

export default createComponent({
  setup () {
    const state = reactive({
      items
    })

    function getIncomplete (items: Item[]): Item[] {
      return items.filter((item: Item) => item.done === false)
    }

    function getComplete (items: Item[]): Item[] {
      return items.filter((item: Item) => item.done)
    }

    function getItemById (id: Number): Item|null {
       const index = state.items.findIndex((item: Item) => item.id === id)

       if (index === -1) {
         return null
       }

       return state.items[index]
    }
 
    const incompleteItems = computed(() => getIncomplete(state.items))
    const completeItems = computed(() => getComplete(state.items))
    const incompleteCount = computed(() => incompleteItems.value.length)
    const completeCount = computed(() => completeItems.value.length)
    const sortedItems = computed(() => [
      ...incompleteItems.value,
      ...completeItems.value
    ])

    function markDone (itemId: Number) {
      const item = getItemById(itemId)

      if (item === null) {
        return
      }

      item.done = true
    }

    function reset () {
      state.items.forEach(item => {
        item.done = false
      })
    }

    return {
      sortedItems,
      incompleteCount,
      completeCount,
      markDone,
      reset
    }
  }
})
</script>

<style scoped>
  .list {
      max-width: 450px;

      background-color: white;
  }

  @media (min-width: 450px) {
      .list {
          border-radius: 8px;
          margin: 0 auto;
      }
  }

  h1 {
      font-size: 1.6em;
      padding: 1.2em;
  }

  ul {
      display: flex;
      flex-direction: column;

      list-style: none;
      position: relative;
  }

  .items-move {
      transition: transform 0.2s ease-out;
  }

  ul.finished li {
      opacity: 0;
  }

  .complete-message {
      visibility: hidden;
      opacity: 0;
      transform: scale(0.9) translateY(-10px);

      text-align: center;
      font-size: 1.4em;
      padding: 2em;

      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;

      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;

      transition: 0.4s all ease-in-out;
  }

  .complete-message h2 {
      margin-bottom: 0.4em;
  }

  .complete-message p {
      color: rgba(0, 0, 0, 0.6);
  }

  ul.finished .complete-message {
      opacity: 1;
      visibility: visible;
      transform: none;
  }

  li {
      padding: 2em;
      display: flex;
      align-items: center;
      justify-content: space-between;
      border-top: 1px dashed rgba(0, 0, 0, 0.2);
      transition: 0.4s opacity ease-out;
  }

  li span {
      flex: 1;
      max-width: 80%;
      overflow: hidden;
      text-overflow: ellipsis;
      margin-right: 1em;
      white-space: nowrap;
  }

  li.done span {
      text-decoration: line-through;
      font-style: italic;
      color: rgba(0, 0, 0, 0.4);
      transition: 0.4s color ease-in-out;
  }

  li:not(.done) span {
      font-weight: bold;
  }

  button {
      background-color: hsl(0, 0%, 85%);
      padding: 0.6em 0.8em;
      font-weight: bold;
      border: 0;
      border-radius: 8px;
      cursor: pointer;
  }

  button:focus,
  button:hover {
      background-color: hsl(0, 0%, 80%);
  }

  button:active {
      background-color: hsl(0, 0%, 75%);
  }

  button.mark-done {
      background-color: hsl(125, 50%, 48%);
      color: white;
  }

  li.done button.mark-done {
      visibility: hidden;
      opacity: 0;
      transition: 0.2s all ease-in-out;
  }

  button.mark-done:focus,
  button.mark-done:hover {
      background-color: hsl(125, 50%, 43%);
  }

  button.mark-done:active {
      background-color: hsl(125, 50%, 40%);
  }

  button.reset {
      margin-top: 2em;
  }
</style>
