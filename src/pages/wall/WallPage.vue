<script>

import Card from "../../components/ui/Card/Card.vue";
import PostForm from "./PostForm.vue";
import {getUrlAndHeaders  } from "./../../../services/fetchOptions";

export default {
  name: "WallPage",
  components: {
    Card,
    PostForm
  },
 methods: {
 redirectToLoginIfNoToken(){
 const token = localStorage.getItem("token")
    if (token == null) {
    this.$router.push("/login")
    }
  
  }
},

  mounted() {
    this.redirectToLoginIfNoToken()
    const {url, headers} = getUrlAndHeaders()
    const options = {
      headers: {...headers}
      }
    



    fetch(url + "posts/", options)
      .then((res) => {
        if (res.status === 200) {
          return res.json()
      
        } else {
          throw new Error("failed to fetch posts")
        }
      })
      .then((res) => {
        const {email,posts} = res 
        this.posts = posts
        this.currentUser = email
        
        
      })
      .catch((err) => console.log("err:", err))
  },
  data() {
    return{
      posts: [],
      currentUser: null
    }
  }
}

</script>
<template>
  
  <div v-if= "currentUser" class = "container-sm">
    <h1>{{currentUser}}'s Wall Page</h1>
    <PostForm></PostForm>
    <div v-if="posts.length === 0">No posts to display, Start Chatting!</div>
    <div v-for="posts in posts">
      
    <Card 
    :currentUser="email"
    :email="posts.user.email"  
    :content="posts.content" 
    :url="posts.imageUrl"
    :comments="posts.comments" 
    :id="posts.id"
        
      >
       </Card>
        
  </div>



</div>

</template>
<style scoped>
h1 {
  font-size: 1rem;
  text-align: center;
  font-weight: bold;
  margin: 0.5rem;
}
</style>