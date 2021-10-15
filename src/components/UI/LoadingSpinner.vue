<template>
    <div class="lds-dual-ring"></div>
</template>

<style scoped>
.lds-dual-ring {
  display: inline-block;
  width: 5px;
  height: 5px;
  margin-left: 1rem;
  padding-bottom: .6rem ;
}
.lds-dual-ring:after {
  content: " ";
  display: block;
  width: 6px;
  height: 6px;
  border-radius: 50%;
  border: 3px solid #fff;
  border-color: #fff transparent #fff transparent;
  animation: lds-dual-ring 1.2s linear infinite;
}
@keyframes lds-dual-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>