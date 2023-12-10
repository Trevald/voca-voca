<template>
  <textarea
    v-model="wordsAsString"
    @input="inputChange"
    autocomplete="off"
    placeholder="Add some words..."
    style="width: 300px; height: 200px; display: block"
  ></textarea>
</template>

<script>
export default {
  name: "Glossaries",

  model: {
    prop: "value",
    event: "input",
  },

  props: {
    words: Array,
  },

  data: () => {
    return {
      wordsAsString: undefined,
    };
  },

  computed: {},

  methods: {
    inputChange() {
      this.$emit("wordsChange", this.wordsAsString.split("\n"));
    },
  },

  mounted() {
    this.wordsAsString = this.words !== undefined ? this.words.join("\n") : "";
  },
};
</script>
