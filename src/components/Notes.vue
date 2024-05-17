<template>
  <div class='notes'>
    <div class='container'>
      <div class='notes__top'>
        <h2 class='notes__title' v-if="search == ''">
          {{ notes.length == 0 ? words.noinfobar[lang] : words.infobar[lang] }}
        </h2>
        <h2 class='notes__title' v-else>
          {{ words.appbarsearch[lang] }}
        </h2>
        <button
          @click='grid = !grid'
          class='notes__btn'
        >
          <img
            src='@/assets/img/bars.svg'
            alt='bars.svg'
            v-show="grid === true"
          />
          <img
            src='@/assets/img/setka.svg'
            alt='setka.svg'
            v-show="grid === false"
          />
          <span>
            {{ grid ? words.list[lang] : words.grid[lang] }}
          </span>
        </button>
      </div>
      <div class='notes__list' :class='{ active: !grid }'>
        <NoteItem
          v-for="note in notes"
          :key="note.id"
          :note="note"
          @remove="$emit('remove', note.id)"
          @changeNote="$emit('changeNote', note.id)"
          :lang="lang"
          :grid="grid"
        />
      </div>
    </div>
  </div>
</template>

<script>
import NoteItem from '@/components/NoteItem.vue'

export default {
  components: {
    NoteItem,
  },
  data() {
    return {
      grid: true,
    }
  },
  props: {
    notes: {
      typeof: Array,
      required: true,
    },
    lang: String,
    search: String
  },
  inject: ['words']
}
</script>

<style>
.notes {
  margin-top: 30px;
}
.notes__top {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 30px;
}
.notes__title {
  font-family: 'RR';
  font-size: 22px;
  color: #323232;
}
.notes__btn {
  width: 120px;
  height: 56px;
  border-radius: 16px;
  box-shadow: 0px 1px 3px 0px rgba(0, 0, 0, 0.3),
    0px 4px 8px 3px rgba(0, 0, 0, 0.15);
  background: linear-gradient(
    0deg,
    rgba(103, 80, 164, 0.11),
    rgba(103, 80, 164, 0.11)),
    #fffbfe;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 12px;
  cursor: pointer;
  padding: 21px 20px;
}
.notes__top span {
  font-family: 'RM';
  font-size: 14px;
  color: #6750a4;
}
.notes__list {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}
.notes__list.active {
  grid-template-columns: 1fr;
}
@media (max-width: 1024px) {
  .notes__list {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
  }
}
@media (max-width: 768px) {
  .notes__list {
    display: grid;
    grid-template-columns: repeat(1, 1fr);
  }
}
@media (max-width: 320px) {
  .notes__list {
    display: grid;
    grid-template-columns: repeat(1, 1fr);
    width: 100%;
  }
}
</style>
