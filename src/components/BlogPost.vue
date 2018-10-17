<template>
  <div class="post__border">
    <div class="post__date">{{ date }}</div>
    <div><p class=post__title>{{ title }}<span class=post__author> by {{ author }}</span></p></div>
    <hr />
    <p class="post__entry">{{ entry }}</p>
    <p class="post__label">label: {{ label }}</p>
    <button v-on:click="deleteItem(title)">remove</button>
  </div>
</template>

<script>
export default {
  name: 'BlogPost',
  props: ['title', 'author', 'entry', 'label', 'date'],
  methods: {
    deleteItem : function (title) {
      this.$emit('deleteItem', title)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.post__container{
  margin: 20px 20px;
  padding: 20px 20px;
  color: white;
  border-radius: 12px;
}

.post__entry {
  text-align: left;
}

.post__label {
  text-align: right;
  font-size: .75em;
}

.post__date {
  text-align: right;
  font-size: .75em;
}

.post__title {
  text-align: left;
}

.post__author {
  font-size: .75em;
}
</style>
