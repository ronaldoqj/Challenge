<template>
  <div id="container__login">
    <div id="box__login">
      <h1>System about shifts</h1>
      <q-btn
        class="btn__access"
        :to="{name:'Shifts'}"
        aria-label="Menu"
        label="Access"
        color="indigo"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: 'Login'
}
</script>
<style lang="scss">
#container__login {
  h1 {
    margin: 0;
    padding: 0;
    font-size: 2em;
    line-height: 1.1em;
  }
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;

  #box__login {
    margin: 0 auto;
    text-align: center;

    .btn__access { margin-top: 40px; }
  }
}
</style>
