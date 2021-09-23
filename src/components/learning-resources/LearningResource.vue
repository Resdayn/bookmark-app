<template>
  <li>
      <div>
          <header>
              <h3>{{ title }}</h3>
              <button>Delete</button>
          </header>
      </div>
      <p>{{ description }}</p>
      <nav>
          <a :href="link">View Resource</a>
      </nav>
  </li>  
</template>

<script>
export default {
    props: ['title', 'description', 'link']
}
</script>

<style scoped>
li {
  margin: 1rem auto;
  padding: 0.3rem 1rem;
  max-width: 40rem;
  border-radius: 25px;
  color: #C5CAD8;
  background-color: #3677A4;
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
  color: #ce5c00;
}

a:hover,
a:active {
  color: #c89300;
}
</style>