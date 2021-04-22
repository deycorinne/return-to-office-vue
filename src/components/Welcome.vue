<template>
  <div id="welcome" v-if="welcome">

    {{ message }}

    <li v-for="state in states">
        <button v-on:click="selectState(state)">
            {{ state.name }}
        </button>
    </li>

  </div>


  <div id="selected" v-if="!welcome">

    {{ selected.name }}

    <button v-on:click="reset">
        Reset
    </button>

  </div>
</template>

<script>
export default {
  name: 'Welcome',
  data() {
    return {
      message: 'Welcome!',
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
