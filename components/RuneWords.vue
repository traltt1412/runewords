<!-- Please remove this file from your project -->
<template>
  <div class="row">
    <div class="col name">{{ data.runeName }} ({{data.combine || data.runes.join("")}})</div>
    <div class="col level">{{ data.level }}</div>
    <div class="col items">
      <div v-for="(item, iIndex) in data.items" :key="iIndex">
        {{ item }}
      </div>
    </div>
    <div class="col runes">{{ data.runes.join(" + ") }}</div>
    <div class="col effects">
      <div v-for="(effect, eIndex) in data.effects" :key="eIndex">
        {{ effect }}
      </div>
    </div>
  </div>
</template>
<style scoped lang='scss'>
.row {
  width: 100%;
  display: grid;
  grid-template-columns: 200px 50px 250px 150px auto;
}

.col {
  padding: 0 16px;
}
</style>
<script>
export default {
  props: {
    data: {
      required: true,
    },
  },
  data() {
    return {
      name: "",
      otherName: "",
      runes: [],
      effects: [],
      items: [],
    };
  },
  mounted() {
  },
};
</script>
