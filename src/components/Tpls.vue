<template>
  <div class="tpls">
    <div>
      <span class="bold">Article ID:</span>
      <span>{{BringPost.id}}</span>
    </div>
    <div>
      <span class="bold">Title:</span>
      <span>{{BringPost.title}}</span>
    </div>
    <div>
      <span class="bold">Content:</span>
      <span class="block">{{BringPost.body}}</span>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';
export interface PostProto {
  body: string;
  id: number;
  title: string;
  userId:number;
}
export default defineComponent({
  name: 'Tpls',
  props: {
    BringPost:{
      type: Object as PropType<PostProto>,
      required: true
    },
  },
  mounted(){
    // console.log(this.BringPost)
  },
  computed:{
  }

});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
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

.bold {
  font-weight: bold;
}
.block {
  display: block;
}
.tpls {
  text-align: left;
  >div {
    margin-bottom: 10px;
  }
}
</style>
