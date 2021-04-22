<template>
  <div id="welcome" v-if="welcome">

    <p class="font-semibold text-3xl">
      Welcome
    </p>

    <p class="font-semibold text-xl">
      Please select your state:
    </p>

    <li v-for="state in states" class="list-none" :key="state.id">
        <button v-on:click="selectState(state)" 
                class="bg-white hover:bg-gray-100 text-gray-800 
                      font-semibold py-2 px-4 border border-gray-400 
                      rounded shadow" >
            {{ state.name }}
        </button>
    </li>

  </div>

  <div id="selected" v-if="!welcome">

    <p class="font-semibold text-3xl">
    {{ selected.name }}
    </p>    
    
    <p class="font-semibold text-xl">
    positiveIncrease {{ selected.positiveIncrease }}
    </p>
    <p class="font-semibold text-xl">
    positiveCasesViral {{ selected.positiveCasesViral }}
    </p>
    <p class="font-semibold text-xl">
    deathIncrease {{ selected.deathIncrease }}
    </p>
    <p class="font-semibold text-xl">
    lasteUpdateEt {{ selected.lastUpdateEt }}
    </p>

    <button v-on:click="reset" 
            class="bg-white hover:bg-gray-100 text-gray-800 
                  font-semibold py-2 px-4 border border-gray-400 
                  rounded shadow" >
        Reset
    </button>

  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      welcome: true,
      selected: null,
      states: [
          { 
            name: 'MD',
            id: 'MD',
            barrier_states: [
                "MD", "DC", "VA"
            ]
          },
          { 
            name: 'DC',
            id: 'DC',
            barrier_states: [
                "DC", "VA"
            ]
          },
          { 
            name: 'VA',
            id: 'VA' ,
            barrier_states: [
                "VA"
            ],
            positiveIncrease: 0,
            positiveCasesViral: 0,
            deathIncrease: 0,
            lastUpdateEt: 0
          }
      ]
    }
  },
  methods: {
    selectState(state) {
      this.selected = state;
      this.welcome = false;
      state.barrier_states.forEach(state => {

        this.getCovidData(state.toLowerCase());

      });
    },
    reset(event) {
        this.welcome = true;
        this.selected = null;
    },
    getCovidData(id) {
      const url = `https://api.covidtracking.com/v1/states/${id}/current.json`;
      console.log(url);
      axios.get(url)
        .then( (response) => {
          console.log(response.data);
          this.selected.positiveIncrease = response.data.positiveIncrease;
          this.selected.positiveCasesViral = response.data.positiveCasesViral;
          this.selected.deathIncrease = response.data.deathIncrease;
          this.selected.lastUpdateEt = response.data.lastUpdateEt;

          console.log(this.selected);
        })
        .catch( (error)  => {
          console.log(error);
        });
    }
  }
}
</script>

<style scoped>
a {
  color: #42b983;
}
</style>
