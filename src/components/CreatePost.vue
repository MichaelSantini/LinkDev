<!--<template>
  <div id="CreatePost">
    <div v-if="isConnected == true">
      <div id="CreatePostBox">
        <p>Créer un post</p>
        <textarea
          placeholder="Saisissez le contenu de votre post ici"
          name=""
          id=""
          cols="80"
          rows="5"
        ></textarea
        ><br /><input type="file" />
        <button @click="btnTest">PUBLIER</button>
      </div>
    </div>
    <div v-else-if="isConnected == false"><h1>Créer un compte</h1></div>
  </div>
</template>

<script>
export default {
  props: {},
  inject: ["logout", "login", "isConnected", "btnAdmin"],
  data: () => ({}),
  methods: {
    btnTest: function() {
      alert("fonctionne");
    },
  },
};
</script>

<style>
#CreatePostBox {
  margin: auto;
  width: 75%;
  border-radius: 5px;
  box-shadow: 0 0 2px 1px grey;
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
</style> -->
