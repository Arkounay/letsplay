<template>
  <div id="app">
    <h1>Let's play something! But Wut???</h1>
    <ul>
      <li v-for="game in games">
        <game v-bind:game="game" v-bind:chosen_game="chosenGame"></game>
      </li>
    </ul>

    <button v-if="active_items > 1" v-on:click="chooseGame">Pick randomly between these {{ active_items }} items!</button>
    <p class="warning" v-if="active_items <= 1">Enable at least two items to be able to choose randomly.</p>
    <!--<button v-on:click="chooseGame">Choose between these {{ games | filterBy enabled=true | count }} items </button>-->
    <div class="winner" v-if="chosenGame && chosenGame.title">Chosen game : <br> <div class="chosen_bottom">{{ chosenGame.title }}</div>

      <div class="steam_code" v-if="chosenGame.steamCode">
        <div class="center">
          Steam code : <pre>{{ chosenGame.steamCode }}</pre>
          <button v-on:click="copy" class="clipboard" data-clipboard-text="{{ chosenGame.steamCode }}">
            {{ copyText }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import Game from './components/Game';
  import Clipboard from 'clipboard';
  new Clipboard('.clipboard');

  export default {
    components: {
      Game
    },
    data () {
      return {
        games: [
          {title: 'Smite', cover: 'static/games/smite.png', enabled: true, steamCode: 'steam://run/386360'},
          {title: 'Overwatch', cover: 'static/games/overwatch.png', enabled: true},
          {title: 'PUBG', cover: 'static/games/pubg.png', enabled: true, steamCode: 'steam://run/578080'},
          {title: 'Worms W.M.D', cover: 'static/games/worms.png', enabled: true, steamCode: 'steam://run/327030'},
          {title: 'RLX', cover: 'static/games/rlx.png', enabled: false}
        ],
        chosenGame: null,
        copyText: 'Copy to clipboard'
      }
    },
    computed: {
      enabled_games: function() {
        return this.games.filter(function (game) { return game.enabled });
      },
      active_items: function() {
        return this.enabled_games.length;
      }
    },
    methods: {
      chooseGame: function () {
        this.chosenGame = this.enabled_games[Math.floor(Math.random() * this.enabled_games.length)];
        this.copyText = 'Copy to clipboard';
      },
      copy: function () {
        this.copyText = 'Copied!';
      }
    },
    filters: {
      count: function (arr) {
        return arr.length;
      }
    }
  }
</script>

<style lang="scss">
html {
  height: 100%;
}

body {
  text-align: center;
}

#app {
  color: #2c3e50;
  max-width: 600px;
  font-family: Source Sans Pro, Helvetica, sans-serif;
  text-align: center;
  margin: 0 auto;
}

.logo {
  width: 100px;
  height: 100px
}

ul {
  list-style-type: none;
  display: block;
  line-height: 0;
  padding: 0;
}

li {
  display: inline-block;
  margin: 5px;
}

$btn-color: #33C3F0;
button {
  display: block;
  margin: 10px auto;
  padding: 10px 20px;
  background-color: $btn-color;
  border: 1px solid $btn-color;
  border-radius: 4px;
  text-transform: uppercase;
  color: white;

  &:hover {
    cursor: pointer;
    background-color: darken($btn-color, 10%);
 }
}
.clipboard {
  padding: 2px 8px;
  font-size: 10px;
  position: relative;
  bottom: 10px;
}

pre {
  display: inline-block;
  background-color: #F0F0F0;
  padding: 2px 5px;
}

.center {
  text-align: center;
}

.winner {
  background-color: #ffffe0;
  padding: 20px 50px;
  display: inline-block;
}

.chosen_bottom {
  font-size: 40px;
}

.warning {
  background-color: #eceefe;
  padding: 10px 25px;
  display: inline-block;
}

.steam_code {
  margin-top: 15px;
}

</style>
