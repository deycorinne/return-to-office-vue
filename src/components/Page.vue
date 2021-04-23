<template>
  <div id="welcome" v-if="welcome">

    <p class="font-semibold text-3xl">
      Welcome!
    </p>

    <p class="text-l container sm inline-block">
      This page is meant for PBSers who are looking forward to returning to the office once the pandemic has
      calmed down. Because of this, only the 3 states* within the DMV are currently included in this project.
    </p>

    <p class="font-semibold text-xl">
      Where do you commute from?
    </p>

    <ul class="list-none" >

      <li v-for="state in states" :key="state.id" class="inline-block">

          <button v-on:click="selectState(state)" 
                  class="bg-white hover:bg-gray-100 text-gray-800 
                        font-semibold py-2 px-4 border border-gray-400 
                        rounded shadow" >
              {{ state.id }}
          </button>

      </li>

    </ul> 

    <p class="text-xs">
      * Yes, I know DC is not technically a state (yet). Hopefully soon!
    </p>

  </div>

  <div id="selected" v-if="!welcome">

    <p class="font-semibold text-3xl">
      So you travel to the office from {{ selected.name }}...
    </p>    
    
    <p class="font-semibold text-xl">
      This means you travel through {{ selected.barrier_states.length }} states to get to the office!
    </p>

    <p class="font-semibold text-xl">
      Here are the most recent Covid-19 stats for the states you have to travel through:
    </p>

    <div class="state-stats" v-for="state in states" :key="state.id" >

      <div class="state container sm border rounded-md border-black" v-show="included(state)">

        <p class="font-bold text-2xl">
          {{ state.name }}
        </p> 

        <p class="font-semibold text-lg" v-show="state.positiveIncrease">
          There have been {{ state.positiveIncrease }} more cases in this state in the last 24 hours.
        </p>

        <p class="font-semibold text-lg" v-show="state.deathIncrease">
          There have been {{ state.deathIncrease }} more deaths in this state in the last 24 hours.
        </p>

        <p class="font-bold text-xl" v-show="state.positiveCasesViral">
          There are currently {{ state.positiveCasesViral }} positive viral cases in this state.
        </p>

        <p class="font-semibold text-sm" v-show="state.lastUpdateEt">
          This data was last updated at {{ state.lastUpdateEt }}.
        </p>

      </div>

    </div>

    <button v-on:click="reset" 
            class="bg-white hover:bg-gray-100 text-gray-800 
                  font-semibold py-2 px-4 border border-gray-400 
                  rounded shadow" >
        Reset
    </button>

  </div>

</template>

<script>
import axios from 'axios';
import _ from 'lodash';

export default {
  data() {
    return {
      welcome: true,
      selected: null,
      states: [
          { 
            name: 'Maryland',
            id: 'MD',
            barrier_states: [
                "MD", "DC", "VA"
            ],
            covid: {
              positiveIncrease: null,
              positiveCasesViral: null,
              deathIncrease: null,
              lastUpdateEt: null,
            }
          },
          { 
            name: 'District of Columbia',
            id: 'DC',
            barrier_states: [
                "DC", "VA"
            ],
            covid: {
              positiveIncrease: null,
              positiveCasesViral: null,
              deathIncrease: null,
              lastUpdateEt: null,
            }
          },
          { 
            name: 'Virginia',
            id: 'VA' ,
            barrier_states: [
                "VA"
            ],
            covid: {
              positiveIncrease: null,
              positiveCasesViral: null,
              deathIncrease: null,
              lastUpdateEt: null,
            }
          }
      ]
    }
  },
  methods: {
    selectState(state) {

      this.selected = state;
      this.welcome = false;
      state.barrier_states.forEach(state => {

        this.getCovidData(state);

      });

    },
    reset() {

        this.welcome = true;
        this.selected = null;

    },
    getCovidData(id) {
      const url = `https://api.covidtracking.com/v1/states/${id.toLowerCase()}/current.json`;

      axios.get(url)
        .then( (response) => {

          let state = _.find(this.states, function(o) { return o.id == id; });

          state.positiveIncrease = response.data.positiveIncrease;
          state.positiveCasesViral = response.data.positiveCasesViral;
          state.deathIncrease = response.data.deathIncrease;
          state.lastUpdateEt = response.data.lastUpdateEt;

        })
        .catch( (error)  => {
          
          console.log(error);

        });
    },
    included(state) {
      console.log('state', state);
      console.log('states', this.states);
      console.log('find', _.find(this.selected.barrier_states, function(o) { return o == state.id; }));
      return _.find(this.selected.barrier_states, function(o) { return o == state.id; });
    }
  }
}
</script>

<style scoped>

button {
    margin: 15px;
}

p, div {
    margin: 15px;
}

</style>
