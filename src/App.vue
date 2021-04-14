<template>
  <div class="wrapper">
    <div class="wrapper-content">
      <section>
        <div class="container">
          <message v-if="message" :message="message" />
          <new-note :note="note" @addNote="addNote" />

          <div class="note-header" style="margin: 36px 0">
            <h1>{{ title }}</h1>

            <search
              :value="search"
              placeholder="Find your note"
              @search="search = $event"
            />

            <div class="icons">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
                :class="{ active: grid }"
                @click="grid = true"
              >
                <rect x="3" y="3" width="7" height="7"></rect>
                <rect x="14" y="3" width="7" height="7"></rect>
                <rect x="14" y="14" width="7" height="7"></rect>
                <rect x="3" y="14" width="7" height="7"></rect>
              </svg>
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
                :class="{ active: !grid }"
                @click="grid = false"
              >
                <line x1="8" y1="6" x2="21" y2="6"></line>
                <line x1="8" y1="12" x2="21" y2="12"></line>
                <line x1="8" y1="18" x2="21" y2="18"></line>
                <line x1="3" y1="6" x2="3" y2="6"></line>
                <line x1="3" y1="12" x2="3" y2="12"></line>
                <line x1="3" y1="18" x2="3" y2="18"></line>
              </svg>
            </div>
          </div>

          <notes
            :notes="notesFilter"
            @remove="removeNote"
            @show="show"
            @close="close"
            :grid="grid"
          />
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import message from "@/components/Message.vue";
import newNote from "@/components/NewNote.vue";
import notes from "@/components/Notes.vue";
import search from "@/components/Search.vue";

export default {
  name: "App",
  components: {
    message,
    newNote,
    notes,
    search,
  },
  data() {
    return {
      title: "Notes App",
      search: "",
      message: null,
      grid: true,
      note: {
        title: "",
        descr: "",
        select: {
          selected: "Normal",
          options: [
            {
              title: "Normal",
              value: "Normal",
            },
            {
              title: "Medium",
              value: "Medium",
            },
            {
              title: "Strong",
              value: "Strong",
            },
          ],
        },
      },
      notes: [
        {
          title: "First Note",
          descr: "Description for first note",
          date: new Date(Date.now()).toLocaleString(),
          priority: "Strong",
          show: false,
        },
        {
          title: "Second Note",
          descr: "Description for second note",
          date: new Date(Date.now()).toLocaleString(),
          priority: "Normal",
          show: false,
        },
        {
          title: "Third Note",
          descr: "Description for third note",
          date: new Date(Date.now()).toLocaleString(),
          priority: "Medium",
          show: false,
        },
      ],
    };
  },
  methods: {
    addNote() {
      let { title, descr, select } = this.note;
      if (title === "") {
        this.message = "title cant be blank!";
        return false;
      }
      this.notes.push({
        title,
        descr,
        date: new Date(Date.now()).toLocaleString(),
        priority: select.selected,
      });
      this.note.title = "";
      this.note.descr = "";
      this.note.select.selected = "Normal";
      this.message = null;
    },
    removeNote(index) {
      this.notes.splice(index, 1);
    },
    show(index) {
      this.notes.indexOf(index, (this.notes[index].show = !this.notes[index].show));
    },
    close(index){
       this.notes.indexOf(index, (this.notes[index].show = false));
    }
  },
  computed: {
    notesFilter() {
      let array = this.notes,
        search = this.search;

      if (!search) return array;

      search = search.trim().toLowerCase();

      array = array.filter(function (item) {
        if (item.title.toLowerCase().indexOf(search) !== -1) {
          return item;
        }
      });

      return array;
    },
  },
};
</script>

<style>
</style>
