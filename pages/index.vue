<template>
  <div>
    <div class="container filter">
      <input type="text" v-model="searchTerm" placeholder="Runes" />
      <input type="text" class="level" v-model="maxLevel" placeholder="Level" />
    </div>
    <div class="container">
      <rune-words
        class="runewords"
        v-for="item in list"
        :key="item.name"
        :data="item"
      />
    </div>
  </div>
</template>
<style scoped lang='scss'>
.filter {
  padding: 16px;

  .level {
    width: 50px;
  }
}
.runewords {
  padding: 24px 0;

  + .runewords {
    border-top: 2px solid;
  }
}
</style>
<script>
import RuneWords from "~/components/RuneWords.vue";
import runewords from "~/data/runewords";
import runes from "~/data/runes";

export default {
  components: { RuneWords },
  data() {
    return {
      searchTerm: "",
      useAnd: true,
      maxLevel: null,
    };
  },
  computed: {
    list() {
      return (runewords || [])
        .map((rw) => {
          return {
            ...rw,
            level: this.getRWLevel(rw.runes),
          };
        })
        .filter((rw) => {
          return (
            this.checkTerm(this.searchTerm, rw.runes) && this.checkLevel(rw.level, this.maxLevel)
          );
        });
    },
  },
  mounted() {},
  methods: {
    getRWLevel(list) {
      let lv = 0;
      list.forEach((item) => {
        const rune = this.getRuneByName(item);
        if (rune.level > lv) {
          lv = rune.level;
        }
      });
      return lv;
    },
    getRuneByName(name) {
      return runes.find((r) => r.name === name);
    },
    checkTerm(term, array) {
      if (!term) return true;
      const lowerCase = array.join("|").toLowerCase().split("|");
      const terms = term.split(" ");
      if (this.useAnd) {
        for (let i = 0; i < terms.length; i++) {
          if (!lowerCase.includes(terms[i].toLowerCase())) {
            return false;
          }
        }
        return true;
      } else {
        for (let i = 0; i < terms.length; i++) {
          if (lowerCase.includes(terms[i].toLowerCase())) {
            return true;
          } else {
            return false;
          }
        }
      }
    },
    checkLevel(runeLevel, maxLevel) {
      if (!maxLevel) return true;
      return runeLevel <= maxLevel;
    },
  },
};
</script>
