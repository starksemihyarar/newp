<template>
  <div>
    <nav>
      <h1>Links</h1>
      <div>
        <ul>
          <li v-for="(link,key) in ordered" :key="key">
            <a :href="link.url" v-if="!link.isHighlight">{{ link.name }}</a>
            <a :href="link.url" v-else :class="{ highlight: high }">{{
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
      <label for="id">order</label>
      <input type="number" id="id" v-model="link.order" />
      <button @click="changeOrder">
        Sıralama
        <template v-if="orderby_asc">
          Asc
        </template>
        <template v-else>
          Desc
        </template>
      </button>
      <button @click="sendfunc">Submit</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      orderby_asc : true,
      links: [
        {
          name: "1",
          url: "1",
          isHighlight: false,
          order : 1,
        },
        {
          name: "2",
          url: "2",
          isHighlight: false,
          order : 2,
        },
      ],
      link: {
        name: "",
        url: "",
        isHighlight: false,
        order : 0,
      },
      high: true,
    };
  },
  computed : {
    ordered(){
      return this.orderby_asc
        ? this.links.sort((a,b) => (a.order > b.order) ? 1 : ((b.order > a.order) ? -1 : 0))
        : this.links.sort((a,b) =>  (a.order < b.order) ? 1 : ((b.order < a.order) ? -1 : 0))

    }
  },
  methods: {
    sendfunc() {
      let after = Object.values(this.links).filter((item) => {
        if(parseInt(item.order) < parseInt(this.link.order))
          return item
      })
      let before = Object.values(this.links).filter((item) => {
        if(parseInt(item.order) >= parseInt(this.link.order))
          return item
      })
      before.map((item) => {
        item.order = item.order + 1
        return item
      })
      var new_arr = []
      new_arr = [...after]
      new_arr[new_arr.length] = {...this.link}
      new_arr.push(...before)
      this.links = new_arr;
    },

    changeOrder(){
      this.orderby_asc = !this.orderby_asc
    }
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
  color: red;
  font-size: 25px;
  font-weight: 700;
  text-decoration: none;
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
