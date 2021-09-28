<template>
  <div>{{listSkill}}</div>
</template>

<script>
// vue 3 composition api imports
import { ref } from "vue";
export default {
  name: "Generate",
  setup() {
    const GENERATE_URL = "http://localhost:3000";
    const listSkill = ref([]);

    // methods to make api call

    async function getListSkill() {
      const res = await fetch(`${GENERATE_URL}/skills`);
      // set listSkill array to the api response
      listSkill.value = await res.json();
    }
    getListSkill();
    return {
      listSkill,
    };
  },
};
</script>
<style scoped></style>
