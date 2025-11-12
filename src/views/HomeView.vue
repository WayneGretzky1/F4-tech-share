<template>
  <div>
    <!-- Header -->
    <div class="header">
      <div class="home-menu pure-menu pure-menu-horizontal pure-menu-fixed">
        <ul class="pure-menu-list">
          <li class="pure-menu-item pure-menu-selected">
            <RouterLink to="/" class="pure-menu-link">Home</RouterLink>
          </li>
        </ul>
      </div>
    </div>

    <!-- Splash -->
    <div class="splash-container">
      <div class="splash">
        <h1 class="splash-head">Should the Minnesota Twins keep Pablo Lopez?</h1>
        <p class="splash-subhead">Consent to this survey and help out fellow Twins fans</p>
        <p>
          <RouterLink to="/survey" class="pure-button pure-button-primary">Consent</RouterLink>
          <RouterLink to="/decline" class="pure-button pure-button-primary">Decline</RouterLink>
        </p>
      </div>
    </div>
  </div>
</template>

<!-- <style scoped src="../assets/home.css"></style> -->
