<template lang="">
  <header>
    <h1>TODO</h1>
    <label for="darkmodeToggle" class="darkmode-switch">
      <input
        type="checkbox"
        id="darkmodeToggle"
        @click="$emit('ChangeTheme')"
      />
      <i class="checkbox-icon"></i>
    </label>
  </header>
</template>
<script>
export default {};
</script>
<style scoped>
header {
  display: grid;
  grid-template-columns: auto 40px;
  grid-template-rows: 40px;
  align-items: center;
  margin-top: 50px;
  color: white;
}

header h1 {
  letter-spacing: 1rem;
  font-weight: bold;
}

.darkmode-switch {
  position: relative;
}

.darkmode-switch input {
  opacity: 0;
}

.checkbox-icon {
  position: absolute;
  inset: 0;
  background-image: url('/icons/icon-moon.svg');
  background-repeat: no-repeat;
  background-position: center;
}

.darkmode-switch input:checked + .checkbox-icon {
  background-image: url('/icons/icon-sun.svg');
}
</style>
