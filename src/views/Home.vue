<template>
  <div class="home">
    <div id="homeHeader">
      <!-- Box Create Post -->
      <div id="CreatePost">
        <!-- Si connecté (affichage box create post) -->
        <div v-if="checkLogin() === true">
          <div id="CreatePostBox">
            <p></p>
            <textarea
              v-model="mess"
              placeholder="Saisissez le contenu de votre post ici"
              name=""
              id="textAreaPost"
              cols="80"
              rows="5"
            ></textarea
            ><br /><input type="file" />
            <button @click="btnPublish">PUBLIER</button>
          </div>
        </div>
        <!-- Si non connecté (affichage phrase non connecté) -->
        <div v-else-if="checkLogin() === false">
          <p style=" color : grey; font-size : 15px">
            Vous devez être
            <button id="btnHeader" @click="linkConnection">
              connecté
            </button>
            pour pouvoir ajouter une publication
          </p>
        </div>
      </div>
      <!-- Box des postes pushé -->
    </div>
    <div id="homeContent">
      <ul style="list-style-type: none;">
        <li v-for="elem in tabPost" :key="elem">
          <Post
            pseudoName=" Jean Michael"
            like="0"
            nbComment="0"
            :text="post.message"
          />
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import Post from "../components/Post";

export default {
  components: { Post },
  inject: ["logout", "login", "checkLogin", "linkConnection"],
  data() {
    return {
      mess: "",
      post: {
        message: "",
        image: "",
      },
      tabPost: [], //tableau qui récuperera les push posts
    };
  },
  methods: {
    // Method pour push un post dans tableau tabPost
    btnPublish: function() {
      this.post.message = this.mess;
      let newPost = {
        message: this.mess,
        image: this.image,
      };
      this.tabPost.push(newPost);
      console.log(this.post.message);
      this.mess = "";
    },
  },
};
</script>

<style scoped>
/* Bouton "se connecter" header */
#btnHeader {
  color: grey;
  border: transparent;
  box-shadow: none;
  background-color: transparent;
  text-decoration: underline;
  font-size: 15px;
}
#btnHeader:hover {
  background-color: grey;
  box-shadow: 0 0 2px 1px grey;
  color: white;
}
#homeHeader,
#homeContent {
  padding: 10px;
  background-color: white;
  border-radius: 5px;
  margin: auto;
  margin-top: 10px;
  margin-bottom: 20px;
  width: 60%;
  box-shadow: 0 0 5px 1px white;
}

#homeContent {
  height: auto;
  overflow: scroll;
  overflow-x: hidden;
  max-height: 47vh;
}
/* design scrollbar */
/* width */
::-webkit-scrollbar {
  width: 5px;
}

/* Track */
::-webkit-scrollbar-track {
  background: #f1f1f1;
}

/* Handle */
::-webkit-scrollbar-thumb {
  background: #888;
}

/* Le handle  */
::-webkit-scrollbar-thumb:hover {
  background: #555;
}
#CreatePostBox {
  margin: auto;
  width: 75%;
  border-radius: 5px;
}
button {
  padding: 5px;
  border: none;
  color: grey;
  box-shadow: 0 0 2px 1px grey;
  border-radius: 3px;
  font-size: 12px;
  font-weight: 600;
}
button:hover {
  cursor: pointer;

  background-color: gray;
  color: white;
}
#textAreaPost {
  resize: none;
  width: inherit;
}
</style>
