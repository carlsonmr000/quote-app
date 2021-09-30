<template>
  <div id="container">
    <div>
      <Filters />
    </div>
    <div>
      <Quotes />
    </div>
  </div>
</template>

<script>
import Filters from "./components/Filters";
import Quotes from "./components/Quotes.vue";

export default {
  name: 'App',
  components: {
    Quotes,
    Filters
  },
  data () {
    return {
    }
  },
  };
</script>

<style>
.container {
  margin: 20px;
}
</style>