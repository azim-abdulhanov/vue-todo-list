<template>
  <header class='header'>
    <Transition name='header__change' mode='out-in'>
      <div class='header__notes' v-if='header'>
        <button
          v-if='lang == "uz"'
          @click='changeLang'
          class='header__lang'
        >
          RU
        </button>
        <button
          v-else
          @click='changeLang'
          class='header__lang'
        >
          UZ
        </button>
        <h1 class='header__title'>
          {{ words.appbartitle[lang] }}
        </h1>
        <button
          @click='header = false'
          class='header__search'
        >
          <img src='@/assets/img/search.svg' alt='search.svg'/>
        </button>
      </div>

      <div class='header__forms' v-else>
        <button
          class='header__back'
          @click='(header = true), (search = "")'
        >
          <img src='@/assets/img/back.svg' alt='back.svg'/>
        </button>
        <input
          type='text'
          v-model='search'
          :placeholder="words.appbarsearch[lang]"
          class='header__input'
        />
        <button
          @click="search = ''"
          class="header__reset"
        >
          <img src="@/assets/img/reset.svg" alt='reset.svg'>
        </button>
      </div>
    </Transition>
  </header>
</template>

<script>
export default {
  data() {
    return {
      header: true,
      search: '',
    };
  },
  methods: {
    changeLang(){
      this.$emit('changeLang', this.lang == 'ru' ? 'uz' : 'ru')
    }
  },
  watch: {
    search(val){
      this.$emit('find', val)
    }
  },
  props: {
    lang: String
  },
  inject: ['words']
}
</script>

<style>
.header {
  box-shadow: 0px 1px 3px 0px rgba(0, 0, 0, 0.3),
    0px 4px 4px 0px rgba(0, 0, 0, 0.25);
  background: #f3edf7;
  padding: 18px 20px;
  height: 64px;
}
.header__notes,
.header__forms {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.header__lang,
.header__search,
.header__back,
.header__reset {
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
  outline: none;
  border: none;
  background: none;
}
.header__title {
  font-family: 'RR';
  font-size: 22px;
  color: #1c1b1f;
}
.header__input {
  width: 90%;
  font-family: 'RR';
  color: #9d9d9d;
  font-size: 16px;
  background: none;
}
.header__change-enter-active,
.header__change-leave-active {
  transition: 0.3s linear;
}
.header__change-enter-from,
.header__change-leave-to {
  opacity: 0;
  transform: translateY(-100%);
}
</style>
