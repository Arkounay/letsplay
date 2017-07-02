<template>
  <a href="#" v-on:click="toggleEnabled" class="game" v-bind:class="{'disabled': !game.enabled, 'chosen': game == chosen_game}">
      <p class="title">{{ game.title }}</p>
      <div class="preview">
        <div class="container">
          <img v-bind:src="game.cover" alt="" width="100">
        </div>
      </div>
  </a>
</template>

<script>
export default {
  props: ['game', 'chosen_game'],
  methods: {
    toggleEnabled: function () {
      this.game.enabled = !this.game.enabled;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
$size: 120px;

a {
  line-height: 1;
  color: black;
  text-decoration: none;
}

.game {
  display: table;
  border: 1px solid #F0F0F0;
  width: $size;
  height: $size;
  transition: all 200ms;
  &:hover {
    border: 1px solid #CCCCCC;
  }
}

p {
  background-color: #FAFAFA;
  margin: 0;
  padding: 5px;
}

.disabled {
  background-color: #5e5e5e;
  text-decoration: line-through;
  filter: grayscale(100%);
}

.chosen {
  background-color: #ffffe0;
  color: #8890dd;
}

</style>
