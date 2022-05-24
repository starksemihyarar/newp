<template>
  <nav>
    <h1>Links</h1>
    <div>
      <ul>
        <li v-for="link in links" :key="link.id">
          <a :href="link.url" v-if="!link.isHighlight">{{ link.name }}</a>
          <a :href="link.url" v-else :class="{ highlight: !isHighlight }">{{
            link.name
          }}</a>
        </li>
      </ul>
    </div>
  </nav>
  <form @submit.prevent>
    <label for=" name">Name</label>
    <input type="text" id="name" v-model="link.name" />

    <label for="url">Url:</label>
    <input type="text" id="url" v-model="link.url" />

    <label for="radio1">Highlight ?</label>
    <input
      type="radio"
      id="radio1"
      :value="true"
      name="highlight"
      v-model="link.isHighlight"
    />

    <label for="radio2">No Highlight ?</label>
    <input
      type="radio"
      id="radio2"
      :value="false"
      name="highlight"
      v-model="link.isHighlight"
    />
    <label for="id">id</label>
    <input type="number" id="id" v-model="link.id" />
    <button @click="toggle">Sırala</button>
    <button @click="sendfunc">Submit</button>
  </form>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      links: [],
      link: {
        id: "",
        name: "",
        url: "",
        isHighlight: false,
      },
    };
  },
  methods: {
    sendfunc() {
      this.links.push({ ...this.link });
    },
    toggle() {
      if (!this.isActive) {
        this.links.sort((a, b) => a.id - b.id);
        this.isActive = true;
      } else {
        this.links.sort((a, b) => b.id - a.id);
        this.isActive = false;
      }
    },
  },
};
</script>

<style>
input {
  display: block;
  padding: 0.5rem 0.7rem;
  margin: 1rem;
  width: 70%;
}
form {
  display: grid;
  align-items: center;
  max-width: 30%;
}
.highlight {
  color: "blue";
  font-size: "25px";
  text-decoration: "none";
}
button {
  padding: 1rem 0;
  margin-bottom: 1rem;
  max-width: 50%;
  font-weight: 700;
}
ul {
  list-style: none;
}
</style>
