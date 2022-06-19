<template>
  <img id="meow" width="500" alt="Ops..." src="../assets/cat.jpeg">

  <div class="hello">
    <h1>Welcome to <b>BitSleep</b>, an online image ___ (fill in please, I am out of words).</h1>
    <p>Hi, I am meow-meow, say something to me</p>
    <input type="text" v-model="greet_text"/>
    <button @click="greet">Greet</button>
  </div>
  <div>
    <img v-if="show_loading" width="50" src="../assets/giphy.gif" alt="loading"/>
    </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data(){
    return {
      greet_text:"",
      show_loading: false,
    }
  },

  methods:{
    async greet(){
      try{
      let request = {
          "template":"default",
          "customization":[
              {"type":"text", "value": this.greet_text},
          ]
      }
      this.show_loading = true;
      let result = await fetch("/api/image/generate", {
        method:"POST",
        body: JSON.stringify(request),
      })
      let resp = await result.json();
      if(resp.status_code !== 0){
        console.log(resp.status_message);
        return
      }
      document.getElementById('meow').src = resp.data.url;
      }catch(e){
        console.log(e);
      }finally{
        this.show_loading=false;
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
button{
  margin: 10px;
}
</style>
