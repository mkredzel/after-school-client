<template>
  <div id="app">
    <Navigation />
    <Classes />
  </div>
</template>

<script>

import Navigation from './components/Navigation'
import Classes from './components/Classes'


export default {
  components: {
    Navigation,
    Classes
  },
  watch: {
    $route: {
      immediate: true,
      handler(to) {
        document.title = to.meta.title || 'After School Classes';
      }
    },
  },
  data() {
    return {
    };
  }
}
</script>