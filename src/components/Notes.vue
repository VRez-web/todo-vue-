<template>
  <div class="notes">
    <div
      class="note"
      :class="{ full: !grid }"
      v-for="(note, index) in notes"
      :key="index"
    >
      <div class="note-header" :class="{ full: !grid }">
        <p @click="openInput(index)">
          <input
            type="text"
            v-show="note.show"
            v-model="note.title"
            @keypress.enter="close(index)"
          />
          {{ note.title }}
          <span :class="priorityClass(note.priority)">{{ note.priority }}</span>
        </p>
        <p style="cursor: pointer" @click="removeNote(index)">x</p>
      </div>
      <div class="note-body">
        <p>{{ note.descr }}</p>
        <span>{{ note.date }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    notes: {
      type: Array,
      required: true,
    },
    grid: {
      type: Boolean,
      required: true,
    },
  },
  methods: {
    removeNote(index) {
      this.$emit("remove", index);
    },
    priorityClass(item) {
      if (item === "Normal") return "normal";
      if (item === "Medium") return "medium";
      if (item === "Strong") return "strong";
    },
    openInput(index) {
      this.$emit("show", index);
    },
    close(index) {
      this.$emit("close", index);
    },
    changeTitle(index, title) {
      this.$emit("changeTitle", index, title);
    },
  },
};
</script>

<style lang="scss">
.notes {
  display: flex;
  justify-content: space-between;
  align-content: center;
  flex-wrap: wrap;
  padding: 40px 0;
}
.note {
  width: 48%;
  padding: 18px 20px;
  margin-bottom: 20px;
  background-color: #fff;
  transition: all 0.25s linear;
  box-shadow: 0 20px 20px rgba(0, 0, 0, 0.2);

  &:hover {
    box-shadow: 0 20px 20px rgba(0, 0, 0, 0.4);
    transform: translate(0, -6px);
  }
  &.full {
    width: 100%;
    text-align: center;
  }
}
.note-header {
  display: flex;
  justify-content: space-between;
  align-content: center;
  h1 {
    font-size: 32px;
  }
  p {
    color: #402caf;
  }
  svg {
    color: #999;
    cursor: pointer;
    &:not(:last-child) {
      margin-right: 12px;
    }
    &.active {
      color: #402caf;
    }
  }
  span {
    border: 1px solid #999;
    padding: 5px;
    margin-left: 10px;
    color: #fff;

    &.normal {
      background-color: #55b535;
    }
    &.medium {
      background-color: #c9ed15;
      color: #000;
    }
    &.strong {
      background-color: #edb015;
    }
  }
  &.full {
    justify-content: center;
    p {
      &:not(:last-child) {
        margin-right: 16px;
      }
    }
  }
}
.note-body {
  p {
    margin: 20px 0;
  }
  span {
    font-size: 14px;
    color: #999;
  }
}
@media screen and (max-width: 768px) {
  .note-header {
    flex-wrap: wrap;
    align-items: flex-start;

    span {
      display: block;
      margin: 10px 0 0px 0;
    }
  }
}
@media screen and (max-width: 480px) {
  .notes {
    padding: 10px 0;
  }
  .icons {
    display: none;
  }
  .note{
    width: 100%;
  }
}
</style>