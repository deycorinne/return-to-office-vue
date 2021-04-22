<template>
  <div id="welcome" v-if="welcome">

    <p class="font-semibold text-3xl">
      Welcome
    </p>

    <p class="font-semibold text-xl">
      Please select your state:
    </p>

    <li v-for="state in states" class="list-none">
        <button v-on:click="selectState(state)" 
                class="bg-white hover:bg-gray-100 text-gray-800 
                      font-semibold py-2 px-4 border border-gray-400 
                      rounded shadow" >
            {{ state.name }}
        </button>
    </li>

  </div>

  <div id="selected" v-if="!welcome">

    <p class="font-semibold text-xl">
    {{ selected.name }}
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
            ]
          }
      ]
    }
  },
  methods: {
    selectState(state) {
      this.selected = state;
      this.welcome = false;
    },
    reset(event) {
        this.welcome = true;
        this.selected = null;
    }
  }
}
</script>

<style scoped>
a {
  color: #42b983;
}
</style>
