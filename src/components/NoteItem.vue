<template>
  <div class='notes__item'>
    <div class='notes__item-top' :class='{ between: !grid }'>
      <h3 class='notes__item-title'>
        {{ note.title }}
      </h3>
      <p class='notes__item-date'>
        {{ note.date }}
      </p>
    </div>
    <p class='notes__item-text'>
      {{ note.text }}
    </p>
    <div class='notes__item-btns'>
      <button
        @click="$emit('changeNote', note.id)"
        class='notes__item-btn edit'
      >
        <img src='@/assets/img/edit.svg' alt='edit.svg'/>
        <span>
          {{ words.editbtn[lang] }}
        </span>
      </button>
      <button
        @click='$emit("remove", note.id)'
        class='notes__item-btn del'
      >
        <img src='@/assets/img/delete.svg' alt='delete.svg'/>
        <span>
          {{ words.deledit[lang] }}
        </span>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    grid: Boolean,
    note: Object,
    lang: String
  },
  inject: ['words']
}
</script>

<style>
.notes__item {
  border-radius: 16px;
  box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15),
    0px 1px 2px 0px rgba(0, 0, 0, 0.3);
  background: #f3edf7;
  padding: 16px;
}
.notes__item-top {
  display: flex;
  justify-content: space-between;
}
.notes__item-title {
  font-family: 'RM';
  color: #1c1b1f;
  font-size: 16px;
  line-height: 24px;
  word-break: break-all;
  letter-spacing: 0.1px;
}
.notes__item-date {
  font-family: 'RR';
  color: #cac4d0;
  font-size: 14px;
  margin-bottom: 16px;
}
.notes__item-text {
  font-family: 'RR';
  color: #49454f;
  font-size: 14px;
  line-height: 20px;
  word-break: break-all;
  margin-bottom: 29px;
}
.notes__item-btns {
  display: flex;
  justify-content: flex-end;
  align-items: center;
}
.notes__item-btn {
  display: flex;
  align-items: center;
  padding: 12px;
  gap: 8px;
  background: transparent;
  border-radius: 5px;
  transition: .4s ease;
}
.notes__item-btn span {
  font-family: 'RM';
  font-size: 14px;
  line-height: 20px;
  text-transform: uppercase;
}
.edit span {
  color: #6750a4;
}
.del span {
  color: #cf1b1b;
}
.edit:hover {
  background: #e6ddff;
}
.del:hover {
  background: #ffd0d0;
}
.between{
  display: flex;
  justify-content: space-between;
}
@media (max-width: 400px) {
  .notes__item-btns {
    flex-wrap: wrap;
    justify-content: flex-start;
  }
}
</style>
