<template>
  <!-- <router-view/> -->
  

  <div id="app">
    <h1>TypeScript in Vue3</h1>

    <div class="demoBox">
      <h2>Counter</h2>
      <p><input type="text" v-model="inputText" /></p>
      <CharCount :params="charCountParams"></CharCount>
      <p><button @click="reset()">Reset</button></p>
    </div>

    <div class="demoBox">
      <h2>Async Article</h2>
      <a class="pager" @click="pagerChange('prev')" href="javascript:">Prev Page</a>
      <a class="pager" @click="pagerChange('next')" href="javascript:">Next Page</a>
      <TplsComp :BringPost="PostProto"></TplsComp>
    </div>
  </div>
</template>


<script lang="ts">
import { defineComponent } from 'vue';
import TplsComp, { PostProto } from '@/components/Tpls.vue';
import CharCount, { CharCountParams } from '@/components/CharCount.vue';

interface State {
  inputText: string;
  asyncList: Array<number>;
  currentIndex:number;
  currentTitle:string;
  currentBody:string;
  currentID:number;
  currentUser:number;
}
export default defineComponent({
  name:'App',
  components: { CharCount, TplsComp },
  data: (): State => {
    return { 
      inputText: '', 
      currentTitle:'',
      currentBody:'',
      currentID:0,
      currentUser:0,
      asyncList:[],
      currentIndex:0,
    }
  },
  methods: {
    reset(){
      this.inputText = ''
    },
    pagerChange(status:string){
      if(status === 'prev') {
        this.currentIndex --
      }
      if(status === 'next') {
        this.currentIndex ++
      }

      if(this.currentIndex < 0) { this.currentIndex = 0}
      if(this.currentIndex > 100) { this.currentIndex = 100}

      console.log(this.currentIndex)
      const objectIndex = this.currentIndex
      const curretObject = this.asyncList.find((el,index)=> index === objectIndex)
      // console.log(curretObject)
      
      let compileObject = JSON.parse(JSON.stringify(curretObject))
      this.currentTitle = compileObject.title;
      this.currentBody = compileObject.body;
      this.currentID = compileObject.id;
      this.currentUser = compileObject.userId;

    }

  },
  computed: {
    charCountParams(): CharCountParams {
      return {
        inputText: this.inputText,
        label: 'Count',
      }
    },
    PostProto(): PostProto {
      return {
        body: this.currentBody,
        title: this.currentTitle,
        id: this.currentID,
        userId:this.currentUser,
      }
    },
  },

  async created(){
    fetch("https://jsonplaceholder.typicode.com/posts")
      .then(async response => {
        const data = await response.json();

        if (!response.ok) {
          const error = (data && data.message) || response.statusText;
          return Promise.reject(error);
        }
        this.asyncList = data;
        this.currentTitle = data[this.currentIndex].title;
        this.currentBody = data[this.currentIndex].body;
        this.currentID = data[this.currentIndex].id;
        this.currentUser = data[this.currentIndex].userId;
        
      })
      .catch(error => {
        console.error("There was an error!", error);
      });
  },

});
</script>

<style lang="scss">
body {
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  max-width: 600px;
  margin: 0 auto;
  text-align: left;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
h1 { text-align:center;}
h2 { margin-top: 0;}

.demoBox {
  padding: 20px;
  background-color: #f5f5f5;
  margin-bottom: 40px;
}

.pager {
  text-decoration: none;
  background-color: #333333;
  display: inline-block;
  vertical-align: middle;
  padding: 5px 10px;
  font-size: 12px;
  margin:0 15px 15px 0;
  transition: all 0.3s ease-in-out;
  &:link { color: #ffffff;}
  &:hover { color: #ffffff; opacity: 0.5;}
  &:active { color: #ffffff;}
  &:visited { color: #ffffff;}
}
</style>
