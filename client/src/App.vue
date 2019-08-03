<template>
  <div id="app">
    <a href="#">
      <nav class="navbar navbar-expand-lg navbar-dark nav-bg" />
    </a>
    <router-view class="container" />
  </div>
</template>

<style>
.nav-bg {
  background: url('https://media3.giphy.com/media/mLRaNfbEXWDKw/giphy.gif?cid=0f0600155d4514e0526367474599fadf&rid=giphy.gif') center/cover no-repeat;
  background-size: 100% 200%;
  background-position-y: 5%;
  height: 6rem;
}
</style>
