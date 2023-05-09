<template>
  <div>
    <h1>{{ info }}</h1>
    <button @click="clicked">Press</button>
  </div>
</template>

<script>
import axios from "axios"

export default {
  data () {
    return {
      info: ""
    }
  },
  methods:{
    async clicked() {
      try {
        const response = await fetch("http://localhost:8000/stream");
        const reader = response.body.getReader();
        let result = "";
        while (true) {
          const { done, value } = await reader.read();
          if (done) {
            break;
          }
          result = new TextDecoder().decode(value);
          this.info = result;
        }
      } catch (error) {
        console.error(error);
      }
    
    },
  },
  mounted () {
    this.info = "Click for Action "
  }
}
</script>
