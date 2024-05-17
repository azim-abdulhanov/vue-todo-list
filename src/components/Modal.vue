<template>
  <Transition name='modal'>
    <div class='modal' @click='$emit("showOrClose")'>
      <div class='modal__block' @click.stop>
        <h2 class='modal__title'>
          {{ edit ? words.editwindownbtn[lang] : words.addbtn[lang] }}
        </h2>
        <form class='modal__inputs' @submit.prevent='send'>
          <label>
            <span>Title</span>
            <input type='text' v-model='user.title'/>
          </label>
          <label>
            <span>Content</span>
            <textarea
              v-model='user.text'
              rows='1'
              required
            ></textarea>
          </label>
          <div class='modal__btns'>
            <button
              @click='$emit("showOrClose")'
              class='modal__btn cancel'
            >
              {{ words.closebtn[lang] }}
            </button>
            <button
              v-if='!edit'
              @submit.prevent='send'
              class='modal__btn add'
            >
              {{ words.addbtn[lang] }}
            </button>
            <button
              v-else
              @click='changeNote'
              class='modal__btn edit'
            >
              {{ words.editbtn[lang] }}
            </button>
          </div>
        </form>
      </div>
    </div>
  </Transition>
</template>

<script>
export default {
  props: {
    edit: Boolean,
    currentId: Number,
    editNote: Object,
    lang: String
  },
  inject: ['words'],
  data() {
    return {
      user: {
        title: '',
        text: '',
        id: this.currentId,
      },
    }
  },
  methods: {
    send() {
      this.$emit('addNote', { ...this.user })
      this.$emit('showOrClose')
      for (const key in this.user) this.user[key] = ''
      this.user.id = localStorage.id++
    },
    changeNote() {
      if (this.user.title != '' && this.user.text != '') {
        const editedNote = {
          id: this.editNote.id,
          title: this.user.title,
          text: this.user.text,
          date: 'Изменено ' + new Date().toLocaleString(),
        }
        this.$emit('editedNote', editedNote)
        this.user.title = ''
        this.user.text = ''
      }
    },
  },
}
</script>

<style>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 10;
  background: rgba(0, 0, 0, 0.35);
  display: flex;
  justify-content: center;
  align-items: center;
  user-select: none;
}
.modal__block {
  max-width: 312px;
  width: 100%;
  padding: 24px;
  border-radius: 28px;
  background: linear-gradient(
    0deg,
    rgba(103, 80, 164, 0.11),
    rgba(103, 80, 164, 0.11)),
    #fffbfe;
}
.modal__title {
  font-family: 'RR';
  font-size: 24px;
  color: #1c1b1f;
  line-height: 32px;
}
.modal__inputs {
  display: flex;
  flex-direction: column;
  gap: 16px;
  margin: 16px 0 24px;
}
.modal__inputs label {
  position: relative;
  text-transform: uppercase;
}
.modal__inputs span {
  position: absolute;
  top: 8px;
  left: 16px;
  font-family: 'RR';
  font-size: 12px;
  color: #6750a4;
  line-height: 16px;
  user-select: none;
}
.modal__inputs input,
.modal__inputs textarea {
  border: none;
  outline: none;
  resize: none;
  border-radius: 4px 4px 0px 0px;
  background: #e7e0ec;
  width: 100%;
  font-family: 'RR';
  color: #49454f;
  padding: 23px 16px 9px;
  border-bottom: 1px solid #1c1b1f;
}
.modal__btns {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  gap: 8px;
}
.modal__btn {
  padding: 10px 12px;
  background: transparent;
  font-family: 'RM';
  font-size: 14px;
  text-transform: uppercase;
  transition: .4s ease;
  border-radius: 5px;
  cursor: pointer;
}
.cancel {
  color: #cf1b1b;
}
.add,
.edit {
  color: #6750a4;
}
.cancel:hover {
  background: #ffd0d0;
}
.add:hover,
.edit:hover {
  background: #e6ddff;
}
.modal-enter-active,
.modal-leave-active {
  transition: 0.3s linear;
}
.modal-enter-from,
.modal-leave-to {
  opacity: 0;
  transform: scale(1.5);
}
</style>
