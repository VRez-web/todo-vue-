<template>
  <div class="new-note">
    <label>Title</label>
    <input v-model="note.title" type="text" />
    <label>Priority:</label>
    <select v-model="note.select.selected" style="margin-bottom: 20px">
      <option
        v-for="(option, index) in note.select.options"
        :value="option.value"
        :key="index"
        @click="note.priority = note.select.selected"
      >
        {{ option.title }}
      </option>
    </select>
    <label>Description</label>
    <textarea v-model="note.descr"></textarea>
    <button @click="addNote" class="btn btnPrimary">New note</button>
  </div>
</template>

<script>
export default {
  props: {
    note: {
      type: Object,
      required: true,
    },
  },
  methods: {
    addNote() {
      this.$emit("addNote", this.note);
    },
  },
};
</script>

<style lang="scss">
.new-note {
  text-align: center;
}
</style>