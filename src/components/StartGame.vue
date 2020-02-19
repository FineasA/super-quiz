<template>
  <transition name="bounce">
    <div class="card">
      <div class="card-header">
        <h3 class="card-title text-center">Are you ready to start?</h3>
      </div>
    </div>
  </transition>
</template>

<script>
export default {};
</script>

<style>
.bounce-enter-active {
  animation: bounce-in 0.5s;
}
.bounce-leave-active {
  animation: bounce-in 0.5s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.5);
  }
  100% {
    transform: scale(1);
  }
}
</style>