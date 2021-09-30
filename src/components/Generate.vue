<template>
  <!-- <div>{{listSkill}}</div> -->
  <div class="generator">
    <section class="hero is-danger is-bold has-text-centered py-6">
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
                  @change="getFilteredApp"
                />
                {{ skill.skill }}
              </label>
            </div>
          </div>
          <!-- {{skillSelected}} -->
        </div>
      </div>
    </section>
    <div class="container">
      <div class="columns is-multiline mt-3">
        <div
          class="column is-one-third"
          v-for="app in appListFiltered"
          :key="app.id"
        >
          <div class="card px-4">
            <header class="card-header">
              <p class="card-header-title  has-text-centered  has-text-danger is-uppercase is-size-5">
                {{ app.app }}
              </p>
            </header>
            <div class="card-content">
              <div class="content has-text-left mb-4">
                <p class="is-size-7">{{ app.instructions }}</p>
                <h4>Skills:</h4>
                <ul v-for="skill in app.skills" :key="skill.id">
                  <li>
                    <strong class="is-size-8 px-3 mr-3">{{ listSkill[skill - 1].skill }}</strong>
                    <p v-if="listSkill[skill-1].options" :set="randSkill = getRandom(listSkill[skill-1].options)">
                          🦮 <a :href="randSkill">{{ randSkill }}</a>

                    </p>
                  </li>
                </ul>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
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
    async function getAppList() {
      const resp = await fetch(`${GENERATE_URL}/apps`);
      appList = await resp.json();
      appListFiltered.value = appList;
    }
    // method to get filtered app list
    function getFilteredApp() {
      appListFiltered.value = [];
      for (const app of appList) {
        const appSkillsArray = app.skills;
        const selectedSkillsArray = skillSelected.value;

        if (hasAllSkills(appSkillsArray, selectedSkillsArray)) {
          appListFiltered.value.push(app);
        }
      }
    }
    function hasAllSkills(appSkills, selectedSkills) {
      return selectedSkills.every((f) => appSkills.includes(f));
    }
    function getRandom(value){
      let keys = Object.keys(value)
      return value[keys[keys.length * Math.random() << 0]]

    }

    getListSkill();
    getAppList();

    return {
      listSkill,
      skillSelected,
      appListFiltered,
      getFilteredApp,
      getRandom
    };
  },
};
</script>
<style scoped>
label {
  font-size: 20px;
  margin: 10px;
}
.card{
  height: 100%;
  width: 100%;
}
a{
  word-break: break-word;
}
</style>
