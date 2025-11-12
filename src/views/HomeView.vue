<template>
  <div>

    <!-- Splash -->
    <div class="splash-container">
      <div class="splash">
        <h1 class="splash-head">Should the Minnesota Twins keep Pablo Lopez?</h1>
        <p class="splash-subhead">Consent to this survey and help out fellow Twins fans</p>
        <p id="button-container">
          <RouterLink to="/survey" class="pure-button pure-button-primary">Consent</RouterLink>
          <RouterLink to="/decline" class="pure-button pure-button-primary">Decline</RouterLink>
        </p>
      </div>
    </div>
  </div>
</template>

<!-- <style scoped src="../assets/home.css"></style> -->



<style scoped>
  .splash-container {
    height: 100vh;
    overflow: auto;
  }

  #button-container {
    margin-bottom: 30px;
    display: flex;
    gap: 10px;
    align-items: center;
    justify-content: center;
  }

  .splash {
    margin-top: 0;
  }

</style>
