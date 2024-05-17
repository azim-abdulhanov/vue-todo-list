<template>
  <button class='add__btn'>
    <img src='@/assets/img/edit.svg' alt='edit.svg'>
  </button>
</template>

<script>
export default {}
</script>

<style>
.add__btn {
  position: fixed;
  right: 30px;
  bottom: 30px;
  padding: 19px;
  box-shadow: 0 4px 8px 3px rgba(0, 0, 0, 0.149);
  border-radius: 16px;
  background: linear-gradient(
    0deg,
    rgba(103, 80, 164, .11),
    rgba(103, 80, 164, .11)),
    #fffbfe;
  filter: drop-shadow(0 1px 3px rgba(0, 0, 0, 0.302));
  cursor: pointer;
}
</style>
