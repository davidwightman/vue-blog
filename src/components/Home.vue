<template>
  <div id="app">
    <div class="contain">
      <h1>Leadership Connect Engineering Blog</h1>

      <div class="new">
        <h2>Add a New Post</h2>
        <form @submit.prevent="addPost">
          <div>
          <input v-model="newTitle" placeholder="title" required/>
          <input v-model="newAuthor" placeholder="author name" required/>
          <input v-model="newEntry" placeholder="entry" class="post_entry--input" required/>
          <select v-model="newLabel" required>
            <option disabled value="">Add a New label</option>
            <option>React</option>
            <option>Angular</option>
            <option>Vue</option>
            <option>Ember</option>
          </select>
          <button type="submit">Add New Post</button>
          </div>
        </form>
      </div>

      <div>
        <h2>Filter</h2>
          <select v-model="selected">
            <option disabled value="">Filter with a label</option>
            <option>React</option>
            <option>Angular</option>
            <option>Vue</option>
            <option>Ember</option>
            <option>All</option>
          </select>

           <select v-model="selectedAuthor">
            <option disabled value="">Filter by author</option>
            <option>David</option>
            <option>Stefan</option>
            <option>Oliver</option>
            <option>Rabeya</option>
            <option>Ryan</option>
            <option>Alex</option>
            <option>Jill</option>
            <option>All</option>
          </select>
      </div>

      <div >
        <BlogPost
          v-on:deleteItem="removePost($event)"
          class="post__container"
          :style="{backgroundColor: post.label === 'React' ? 'purple' : (post.label === 'Angular' ? 'red' : (post.label === 'Ember' ? 'grey' : 'green'))}"
          v-for="post in filteredByLabel"
          :key="post.title"
          :title="post.title"
          :author="post.author"
          :entry="post.entry"
          :label="post.label"
          :date="post.date"
        ></BlogPost>
         <!-- <button class="btn btn-xs btn-danger" @click="removePost(post)">Remove Post</button> -->
         
        <h1 v-if="this.filteredPosts.length === 0"
        :style="{color: 'black'}">No results. Please adjust the filters to read another blog post.</h1>
      </div>
      
    </div>
  </div>
</template>

<script>
import BlogPost from '../components/BlogPost.vue'
export default {
  name: 'Home',
  el: '#app',
  components: { BlogPost },
  data() {
    return {
      selected: '',
      selectedAuthor: '',
      filteredPosts: [{}],
      posts: [
        {
          author: 'Ryan',
          title: 'Quod Ducimus Omnis',
          entry: 'Vivamus eu nisl rhoncus, porttitor nibh sit amet, gravida augue. Fusce tristique sit amet eros vel tincidunt. Sed accumsan fermentum rutrum. Ut commodo nisi ut ultrices imperdiet. Nullam justo dui, viverra et nisl a, lacinia auctor sem. In risus nisl, pulvinar convallis fermentum id, mollis condimentum sem. Donec sed scelerisque felis. Nam in eros purus. Integer vehicula nunc viverra nisl euismod rutrum. Mauris purus ante, ornare quis tellus id, gravida dictum nisi.',
          label: 'React',
          date: '9/03/2018'
        },
        {
          author: 'Alex',
          title: 'Vero Eius Laboriosam Ex Repudiandae',
          entry: 'Pellentesque et eros sit amet lectus iaculis ultrices et a risus. Fusce metus velit, eleifend at rhoncus et, convallis vel dui. Phasellus gravida leo quis dui aliquam, in dignissim mauris fermentum. Nunc vehicula nisl tortor, eget rutrum orci tempor id. Mauris auctor turpis a lorem bibendum fringilla. Aliquam erat volutpat. Sed efficitur quam a finibus placerat. Donec consectetur nibh at enim porttitor vehicula. Fusce quis pulvinar lorem. Ut orci magna, accumsan a enim vitae, euismod tincidunt arcu. Sed vulputate porttitor egestas. Donec nibh ante, scelerisque at urna sed, dapibus sodales ante. Etiam ultricies justo at tempus condimentum. Morbi consequat consequat orci, non egestas tortor interdum sed.',
          label: 'Vue',
          date: '6/26/2018'
        },
        {
          author: 'Oliver',
          title: 'Magnam Odit',
          entry: 'Duis nec volutpat metus. Ut tincidunt magna ut semper rhoncus. Pellentesque vel malesuada tellus, at aliquet arcu. Nam neque tellus, malesuada in purus sit amet, viverra tristique ex. Vestibulum consequat felis blandit neque aliquam viverra. Nunc in molestie elit, ut molestie mauris. Quisque non justo bibendum, interdum magna ut, ornare felis.',
          label: 'Angular',
           date: '5/12/2018'
        },
        {
          author: 'Jill',
          title: 'Velit Mollitia Voluptates Assumenda',
          entry: 'Duis nec volutpat metus. Ut tincidunt magna ut semper rhoncus. Pellentesque vel malesuada tellus, at aliquet arcu. Nam neque tellus, malesuada in purus sit amet, viverra tristique ex. Vestibulum consequat felis blandit neque aliquam viverra. Nunc in molestie elit, ut molestie mauris. Quisque non justo bibendum, interdum magna ut, ornare felis.',
          label: 'Ember',
           date: '2/12/2018'
        },
        {
          author: 'Rabeya',
          title: 'Eum Commodi Cupiditate',
          entry: 'Duis nec volutpat metus. Ut tincidunt magna ut semper rhoncus. Pellentesque vel malesuada tellus, at aliquet arcu. Nam neque tellus, malesuada in purus sit amet, viverra tristique ex. Vestibulum consequat felis blandit neque aliquam viverra. Nunc in molestie elit, ut molestie mauris. Quisque non justo bibendum, interdum magna ut, ornare felis.',
          label: 'React',
           date: '6/16/2017'
        },
        {
          author: 'Stefan',
          title: 'Amet',
          entry: 'Quisque vitae mauris nibh. Quisque et lacinia neque. Sed gravida, sem eu pulvinar cursus, dui leo dignissim lacus, sit amet condimentum neque turpis ut orci. Quisque viverra, ipsum et ullamcorper rhoncus, dui diam fermentum augue, a fermentum purus lacus id libero. Nullam eleifend sem et ipsum facilisis, in rutrum diam gravida. Donec vitae nibh fermentum, interdum orci quis, gravida sem. Nam in orci maximus, euismod leo posuere, fringilla sapien. Cras gravida lectus velit, bibendum efficitur mauris elementum et. Donec nec lorem ex. Etiam ac odio metus. Sed sed lacus accumsan, scelerisque augue luctus, posuere erat.',
          label: 'Vue',
          date: '5/08/2017'
        },
        {
          author: 'David',
          title: 'Voluptatem Fuga Cum Asperiores Error',
          entry: 'Aenean pellentesque sed lorem non ultrices. Aenean interdum felis et tortor ultrices elementum. Donec odio neque, consequat nec eros id, suscipit mollis diam. Donec urna est, sagittis ut porttitor non, ornare vel ligula. Duis porttitor eros id mi porttitor, porta dignissim odio laoreet. Vivamus a faucibus lectus. Pellentesque pharetra elit vel elit scelerisque lobortis. Maecenas vitae lectus tempus, fringilla erat at, accumsan odio. Phasellus congue imperdiet orci, non malesuada nisi pretium eget. Etiam vitae augue eget nulla condimentum eleifend. Ut ante neque, iaculis eget facilisis non, congue vel sem.',
          label: 'React',
          date: '01/22/2017'
        },
        {
          author: 'Ryan',
          title: 'Ipsa Consectetur Rerum Repellat Quia',
          entry: 'Fusce id nibh venenatis dolor porttitor semper sit amet sagittis elit. Ut risus ex, molestie eleifend lacinia vel, suscipit quis felis. Cras congue ex arcu, sed pretium magna fringilla at. Phasellus non pretium urna. Vestibulum elementum turpis ac nunc faucibus, quis dignissim dolor euismod. Sed pulvinar mollis nibh. Mauris et nibh finibus, consectetur lacus a, scelerisque enim. Morbi id ante vestibulum, molestie dui eleifend, aliquet enim. Fusce facilisis id arcu rutrum dignissim. Etiam vel massa non nisl eleifend porta. Interdum et malesuada fames ac ante ipsum primis in faucibus. Maecenas nec varius orci.',
          label: 'Vue',
          date: '12/01/2016'
        },
        {
          author: 'Alex',
          title: 'Dolor Nihil Delectus',
          entry: 'Vivamus scelerisque nisi orci, nec ultrices orci vulputate sit amet. Suspendisse in ligula lacus. Morbi sagittis eleifend aliquam. Sed nec mi ut augue lobortis tempor. Etiam egestas libero ac tincidunt lacinia. Fusce molestie risus non tincidunt ultrices. Cras fermentum diam ut arcu egestas, sit amet aliquam est volutpat. Cras pellentesque libero at facilisis fringilla. Fusce varius magna et justo luctus, in hendrerit erat pretium. In dapibus ultricies ultrices. Nam sit amet semper erat, eget consectetur arcu. Proin congue sem nec diam ullamcorper, sit amet ultricies eros vulputate. Vivamus luctus dui erat, sed ultricies eros venenatis vel.',
          label: 'Angular',
          date: '11/03/2016'
        },
        {
          author: 'Oliver',
          title: 'Labore Ipsum Ad Pariatur',
          entry: 'Aenean mollis facilisis nibh non luctus. Aenean turpis ex, tincidunt vel sem sed, aliquam euismod dolor. Nullam mattis ipsum id nisi tempor semper. Vivamus ligula tellus, tincidunt ut cursus facilisis, malesuada quis elit. Etiam sed dolor ac felis ornare rutrum. In eget enim eu nunc convallis maximus. Suspendisse eros est, iaculis ut arcu sit amet, hendrerit ullamcorper sapien. Ut erat lacus, posuere eget posuere at, gravida non turpis. Curabitur mattis scelerisque arcu, ac tempor erat. Suspendisse potenti. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae;',
          label: 'Angular',
          date: '9/02/2016'
        }
      ],
      newTitle: '',
      newAuthor: '',
      newLabel: '',
      newEntry: '',
      newDate: new Date().toLocaleDateString("en-US"),
    }
  },
  methods: {
    addPost() {
      let addedPost = new Object({
        author: this.newAuthor,
        title: this.newTitle,
        entry: this.newEntry,
        label: this.newLabel,
        date: this.newDate,
      });
      this.posts.unshift(addedPost)
      this.newTitle = '';
      this.newEntry = '';
      this.newAuthor = '';
      this.newLabel = '';
    },
    removePost(title) {
      let index;
      this.posts.map(post => {
        if (post.title === title) {
          console.log(this.posts.indexOf(post))
          index = this.posts.indexOf(post)
        };
      });
      if (index > -1) {
        this.posts.splice(index, 1);
      }
    }
  },
  computed: {
    filteredByLabel() {
      let filter = new RegExp(this.selected, 'i');
      let filterSelectedAuthor = new RegExp(this.selectedAuthor, 'i');
    
      if ((this.selected === 'All' || this.selected === '') && (this.selectedAuthor === 'All' || this.selectedAuthor === '')) {
        this.filteredPosts = this.posts;
        return this.posts;
      } else if ((this.selected === 'All' || this.selected === '') && (this.selectedAuthor !== 'All' || this.selectedAuthor !== '')) {
        this.filteredPosts = this.posts.filter(el => el.author.match(filterSelectedAuthor));
        return this.posts.filter(el => el.author.match(filterSelectedAuthor));
      } else if ((this.selected !== 'All' || this.selected !== '') && (this.selectedAuthor === 'All' || this.selectedAuthor === '')){
        this.filteredPosts = this.posts.filter(el => el.label.match(filter));
        return this.posts.filter(el => el.label.match(filter));
      } else if ((this.selected !== 'All' || this.selected !== '') && (this.selectedAuthor !== 'All' || this.selectedAuthor !== '')) {
        this.filteredPosts = this.posts.filter(el => el.author.match(filterSelectedAuthor)).filter(el => el.label.match(filter));
        return this.posts.filter(el => el.author.match(filterSelectedAuthor)).filter(el => el.label.match(filter));
      } 
    },
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

.post__title {
  text-align: left;
}

.post__author {
  font-size: .75em;
}
</style>

