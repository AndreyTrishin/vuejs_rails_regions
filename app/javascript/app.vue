<template>
  <div id="app">
    <p>{{ message }}</p>
    <table class="table">
      <tr v-for="region in message" v-bind:key="region.id">
        <td>{{region.name}}</td>
        <td>{{region.population}}</td>
        <td>
          <input type="button" v-on:click="removeRegion(region.id)" value="Удалить" />
        </td>
      </tr>
    </table>
    <input type="text" v-model="newRegionName" />
    <input type="number" v-model="newRegionPopulation" />
    <input
      type="button"
      v-on:click="addRegion(newRegionName, newRegionPopulation)"
      value="Добавить"
    />
  </div>
</template>

<script>
import Vue from "vue/dist/vue.esm";
import VueResource from "vue-resource";
Vue.use(VueResource);
var app = {
  props: ["message", "newRegionName", "newRegionPopulation"],
  methods: {
    addRegion(newRegionName, newRegionPopulation) {
      this.$http
        .post(
          "/regions?region[name]=" +
            newRegionName +
            "&region[population]=" +
            newRegionPopulation
        )
        .then(res => {
          console.log(res);
          location.reload();
        })
        .catch(ex => {
          console.log(ex);
        });
    },
    removeRegion(regionId) {
      if (confirm("Are you sure")) {
        this.$http
          .delete("regions/" + regionId)
          .then(res => {
            console.log(res);
            location.reload();
          })
          .catch(error => console.log("Got a problem" + error));
      }
    }
  }
};
console.log(app.props["message"]);

export default app;
</script>

<style scoped>
p {
  text-align: center;
}
</style>
