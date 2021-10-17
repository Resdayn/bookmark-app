<template>
  <li>
      <base-card>
          <header>
              <h3>{{ title }}</h3>
              <base-button mode="flat" @click="removeResource(id)">Delete</base-button>
          </header>
          <p>{{ description }}</p>
          <nav>
              <a :href="link"><img src="../../../public/icons/arrow-right.png"> Visit site</a>
          </nav>
      </base-card>
  </li>  
</template>

<script>
export default {
  props: ['id', 'title', 'description', 'link'],
  inject: ['removeResource']
}
</script>

<style scoped>
li {
  margin: auto;
  max-width: 40rem;
  color: white;

}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h3 {
  font-size: 1.25rem;
  margin: 0.5rem 0;
}

p {
  margin: 0.5rem 0;
}

a {
  text-decoration: none;
  color: white;
  display: flex;
  place-items: center;
}

a:hover,
a:active {
  color: #b8b19f;
}

img {
  transform: scale(0.8);
  margin-right: 0.2rem;
}
</style>