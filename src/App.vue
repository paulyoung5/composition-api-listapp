<template>
    <div id="app">
        <list />
    </div>
</template>

<script lang="ts">
import Vue from 'vue'
import { createComponent } from '@vue/composition-api'
import List from './components/List.vue'

export default createComponent({
  name: 'app',
  components: {
    List
  }
})
</script>

<style>
    * {
        box-sizing: border-box;
        padding: 0;
        margin: 0;
    }

    html, body {
        height: 100vh;
        overflow: hidden;
    }

    body {
        font: 16px sans-serif;

        background-color: rgba(0, 0, 0, 0.1);
    }

    @media (min-width: 450px) {
        body {
            padding: 3em;
        }
    }
</style>
