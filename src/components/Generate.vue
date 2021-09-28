<template>
  <!-- <div>{{listSkill}}</div> -->
  <div class="generator">
    <section class="hero is-primary is-bold has-text-centered py-6">
      <div class="hero-body">
        <div class="container">
          <h1 class="title">
            Hey, what skills would you love to work on?
          </h1>
          <div v-for="skill in listSkill" :key="skill.id">
            <div class="field">
              <label class="checkbox">
                <input
                  type="checkbox"
                  v-model="skillSelected"
                  :value="skill.id"
                />
                {{ skill.skill }}
              </label>
            </div>
          </div>
          <!-- {{skillSelected}} -->
        </div>
      </div>
    </section>
  </div>
</template>

<script>
// vue 3 composition api imports
import { ref } from "vue";
export default {
  name: "Generate",
  setup() {
    const GENERATE_URL = "http://localhost:3000";
    const listSkill = ref([]);
    const skillSelected = ref([]);
    // for app ideas
    const appListFiltered = ref([]);
    // appList isnt exposed  in the template hence ref() aint used
    let appList = [];

    // methods to make api call

    async function getListSkill() {
      const res = await fetch(`${GENERATE_URL}/skills`);
      // set listSkill array to the api response
      listSkill.value = await res.json();
    }


    // method to get app idea list
    async function getAppList(){
      const resp = await fetch(`${GENERATE_URL}/apps`)
      appList=  await resp.json()
      appListFiltered.value = appList
    }
    getListSkill();
    getAppList()
    return {
      listSkill,
      skillSelected,
      appListFiltered
    };
  },
};
</script>
<style scoped>
label {
  font-size: 20px;
  margin: 10px;
}
</style>
