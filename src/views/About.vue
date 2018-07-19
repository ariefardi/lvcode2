<template>
<div>
    <input type="file" @change="postImageHandler">
    <v-btn @click="upload" to="/about" >upload</v-btn>
    <v-container>
      <v-layout row wrap>
    <v-flex xs6>
    <v-card v-for="(img, index) in result" :key="index">
      <img style="max-width:100%" :src="img.url" alt="">
    </v-card>
    </v-flex>
    </v-layout>
    </v-container>
</div>
</template>

<script>
import axios from 'axios'
export default {
  created () {
    this.getPost()
    let token = localStorage.getItem('token')
    if (!token) {
      this.$router.push('/')
    }
  },
  data () {
    return {
      result: [],
      img: ''
    }
  },
  methods: {
    postImageHandler (e) {
      this.img = e.target.files[0]
    },
    getPost () {
      console.log('axios get kok gak jalan ya')
      let uuid = localStorage.getItem('token')
      axios.get('http://35.240.157.177/image', {
        headers: {
            authorization: uuid
        }
      })
      .then(({data})=> {
        console.log(data, ' ini data')
        this.result = data.reverse()

      })
    },
    upload () {
      console.log('upload')
      console.log(this.img)
      let formData = new FormData()
      formData.append('file', this.img)
      let uuid = localStorage.getItem('token')
      axios({
          url: 'http://35.240.157.177/image',
          method: 'post',
          data: formData,
          headers: {
              authorization: uuid
          }
      })
      .then(({data})=> {
        console.log(data, 'ini data dari upload')
        this.result.push(data)
      })
    }
  }
}
</script>

