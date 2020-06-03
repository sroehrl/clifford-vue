<template>
  <div class="home">
    <div>
      <input type="text" v-model="zip">
      <button @click="askBackend">check zip-code</button>
    </div>
    <div v-if="dealerFound">
      <h2>Your dealer: {{dealer.name}}</h2>
      <p>located at {{dealer.postal_code}}</p>
    </div>
    <p v-if="warning">
      NO DEALER
    </p>

  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue';
import axios from 'axios'

export default {
  name: 'Home',
  data:function(){
    return {
      zip:'',
      dealerFound: false,
      warning:false,
      dealer:{}
    }
  },
  methods:{
    async askBackend(){
      try{
        let beZip = await axios.get('http://localhost/clifford-api/api.v1/zip?zip='+this.zip)
        if(beZip.data.length > 0){
          this.dealerFound = true;
          this.dealer = beZip.data[0];
        } else {
          this.warning = true;
        }
      } catch (e) {
        alert('backend unavailable')
      }


    }
  },
  components: {
    HelloWorld
  }
}
</script>
