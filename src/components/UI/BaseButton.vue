<template>
    <button :type="type" :class="mode">
        <slot></slot>
    </button>
</template>

<script>
export default ({
    props: ['type', 'mode']
})
</script>


<style scoped>
button {
  padding: 0.75rem 1.5rem;
  font-family: inherit;
  background-color: #5FA5D3;
  border: 1px solid #ffffff;
  color: white;
  cursor: pointer;
}

button:hover,
button:active {
  background-color: #5394be;
  border-color: #5394be;
}

/* Extra flat class for an alternative style */
.flat {
  background-color: transparent;
  color: #ffffff;
  border: none;
}

.flat:hover,
.flat:active {
  background-color: #5394be;
}
</style>