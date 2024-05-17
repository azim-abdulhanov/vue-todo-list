<template>
  <Navbar
    @find='search = $event'
    :lang='lang'
    @changeLang='changeLang'
  />
  <Notes
    :search='search'
    :notes='filterNotes'
    @remove='removeNote'
    @changeNote='changeNote'
    :lang='lang'
  />
  <Modal
    v-show='showModal'
    @showOrClose='showModal = false'
    @addNote='addNote'
    :edit='edit'
    :editNote='editNote'
    :currentId='currentId'
    @editedNote='editedNote'
    :lang='lang'
  />
  <AddButton @click='showModal = !showModal, edit = false' />
</template>

<script>
import Navbar from '@/components/Navbar.vue'
import Notes from '@/components/Notes.vue'
import Modal from '@/components/Modal.vue'
import AddButton from '@/components/AddButton.vue'
import langs from '@/lang.js'

export default {
  components: {
    Navbar,
    Notes,
    Modal,
    AddButton
  },
  data() {
    return {
      notes: [],
      showModal: false,
      edit: false,
      editNote: {},
      currentId: localStorage.id ? localStorage.id : localStorage.id = 0,
      lang: 'ru',
      langWords: {},
      search: '',
    }
  },
  methods: {
    addNote(obj) {
      const note = { ...obj }
      note.date = new Date().toLocaleString()
      note.id = ++this.currentId
      this.notes.push(note)
    },
    removeNote(id) {
      const idx = this.notes.findIndex(c => c.id == id)
      this.notes.splice(idx, 1)
    },
    getNotes() {
      this.notes = JSON.parse(localStorage.notes)
    },
    changeNote(id) {
      this.edit = this.showModal = true;
      let pickedNote = this.notes.find(note => note.id == id)
      this.editNote = pickedNote
    },
    editedNote(noteEdited) {
      this.notes.forEach(note => {
        if (note.id == noteEdited.id) {
          note.title = noteEdited.title
          note.text = noteEdited.text
          note.date = noteEdited.date
        }
      })
      this.showModal = false
    },
    changeLang(val) {
      this.lang = localStorage.lang = val
    },
  },
  watch: {
    notes: {
      handler() {
        localStorage.notes = JSON.stringify(this.notes)
      },
      deep: true,
    },
  },
  computed: {
    filterNotes() {
      if (this.search) {
        let searchItem = this.search.toLowerCase()
        const filterNotes = this.notes.filter(item => {
          const title = item.title.toLowerCase()
          const date = item.date.toLowerCase()
          const text = item.text.toLowerCase()
          if (title.includes(searchItem)) return item
          if (date.includes(searchItem)) return item
          if (text.includes(searchItem)) return item
        })
        return filterNotes
      }
      else return this.notes
    },
  },
  created() {
    if (localStorage.notes) {
      this.getNotes()
    }
    localStorage.lang = localStorage.lang || 'ru'
    this.lang = localStorage.lang
    this.langWords = langs
    localStorage.words = JSON.stringify(this.langWords)
  },
  provide() {
    return {
      words: localStorage.words ? JSON.parse(localStorage.words) : location.reload(),
    }
  },
}
</script>

