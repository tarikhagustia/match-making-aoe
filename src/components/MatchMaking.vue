<template>
  <div class="mb-5">
    <div class="header-bg row align-items-center text-center text-gold">
      <div>
        <h1 class="text-sm">Age Of Empires Indonesia</h1>
        <p class="text-sm">Match Making</p>
      </div>
    </div>
   
    <div class="container">
      <div class="row mt-5">
        <div class="col-sm-4">
          
        </div>
      </div>
      <form v-if="teams.a.length == 0">
        <label class="text-gold">Select Player</label>
        <select
          class="form-control"
          v-model="players"
          multiple
          style="min-height: 400px"
        >
          <option :value="item" v-for="(item, k) in items" :key="k">
            {{ item.name }} - {{ item.elo }}
          </option>
        </select>
        <button
          type="button"
          class="btn btn-primary mt-2"
          @click.prevent="buildTeamButtonPressed"
        >
          Build Team
        </button>
      </form>

      <div class="row mt-3 text-gold" v-if="teams.a.length > 0">
        <a href="#" @click.prevent="resetTeam">Reset</a>
        <div class="col-sm-6">
          <h3>Team A</h3>          
            <div v-for="(p, k) in teams.a" :key="k">
              <player :name="p.name" :elo="p.elo"></player>
            </div>
          <p>Total Elo {{ countElo(teams.a) }}</p>
        </div>

        <div class="col-sm-6">
          <h3>Team B</h3>
          <div v-for="(p, k) in teams.b" :key="k">
              <player :name="p.name" :elo="p.elo"></player>
            </div>

          <p>Total Elo {{ countElo(teams.b) }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Player from "./Player.vue";

function dynamicSort(property) {
  var sortOrder = 1;
  if (property[0] === "-") {
    sortOrder = -1;
    property = property.substr(1);
  }
  return function (a, b) {
    /* next line works with strings and numbers,
     * and you may want to customize it to your needs
     */
    var result =
      a[property] < b[property] ? -1 : a[property] > b[property] ? 1 : 0;
    return result * sortOrder;
  };
}

export default {
  name: "HelloWorld",
  components: {Player},
  data() {
    return {
      players: [],
      items: [],
      teams: {
        a: [],
        b: [],
      },
    };
  },
  mounted() {
    // Fetch data
    fetch("/players.json")
      .then((response) => response.json())
      .then((data) => (this.items = data));
  },
  methods: {
    resetTeam() {
      // Clear Teams
      this.teams.a = [];
      this.teams.b = [];
    },
    buildTeamButtonPressed() {
      // Clear Teams
      this.teams.a = [];
      this.teams.b = [];

      // Randomize
      this.players = this.players.sort(() => Math.random() - 0.5);

      // Sort By Elo
      this.players = this.players.sort(dynamicSort("-elo"));

      this.players.forEach(async (item) => {
        let sum = 0;
        let sumB = 0;
        for (var i = 0; i < this.teams.a.length; i++) {
          sum += this.teams.a[i].elo;
        }

        for (var j = 0; j < this.teams.b.length; j++) {
          sumB += this.teams.b[j].elo;
        }
        if (sum > sumB) {
          this.teams.b.push(item);
        } else {
          this.teams.a.push(item);
        }
      });
    },

    countElo(team) {
      let sum = 0;
      for (var i = 0; i < team.length; i++) {
        sum += team[i].elo;
      }

      return sum;
    },
  },
};
</script>