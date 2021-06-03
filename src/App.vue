<template>
  <div id="app">
    <!-- DIV CONNECTION -->
    <!-- DIV CONNECTION -->
    <!-- DIV CONNECTION -->
    <div id="connection">
      <!-- si divConnexion est true affiche la div -->
      <div v-if="divConnexion == true" id="fondGris">
        <div id="divConnexion" class="box">
          <div class="connect">
            <!-- Boite générale de "Connexion" -->
            <div id="boxConnect">
              <!-- Bouton pour quitter la page connexion -->
              <button @click="closeConnexion" id="buttonLeaveConnection">
                <font-awesome-icon icon="times" />
              </button>

              <!-- Header de "Connexion" avec titre -->
              <div id="headerConnect"><h3>Connexion</h3></div>

              <!-- Contenu de "Connexion" avec input etc -->
              <div id="contentConnect">
                <label for="inputConnect">E-mail :</label><br />
                <input
                  v-model="logMail"
                  class="inputConnectSubscribe"
                  type="text"
                  placeholder="Mail"
                /><br /><br />
                <label for="inputPassword">Mot de passe :</label><br />
                <input
                  v-model="logPassword"
                  class="inputConnectSubscribe"
                  type="password"
                  placeholder="Mot de passe"
                />
              </div>

              <!-- footer de "Connexion" avec bouton etc -->
              <div id="footerConnect">
                <!-- .Attention, Home = Link modelé en btn -->
                <br /><button class="btnConnectSubscribe" @click="validLogin">
                  login
                </button>
                <p style="font-style : italic; color:grey; font-size :15px">
                  Pas de compte?
                  <button
                    class="btnConnectSubscribe"
                    @click="connectionToSubscribe"
                  >
                    Inscrivez vous !
                  </button>
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- DIV SUBSCRIBE -->
    <!-- DIV SUBSCRIBE -->
    <!-- DIV SUBSCRIBE -->

    <!-- si divsubscribe est true affiche la div -->
    <div v-if="divSubscribe == true" id="fondGris">
      <div id="divSubscribe" class="box">
        <div id="subscribe">
          <!-- Bouton fermer la fenetre inscription -->
          <button @click="closeSubscribe" id="buttonLeaveSubscribe">
            <font-awesome-icon icon="times" />
          </button>

          <h3>Inscription</h3>
          <div id="pseudo">
            <!-- pseudo -->
            <label for="pseudo">Pseudo</label><br />
            <input
              type="text"
              name="pseudo"
              class="inputConnectSubscribe"
              placeholder="Ex : MMLdev"
              v-model="user.pseudo"
            /><br />
          </div>
          <!-- mail -->
          <div id="divEmail">
            <label for="email">E-mail</label><br />
            <input
              type="text"
              name="email"
              placeholder="Ex : MMLdev@gmail.com"
              class="inputConnectSubscribe"
              v-model="user.email"
            />
          </div>
          <div>
            <!-- password -->
            <label for="password">Mot de passe</label><br />
            <input
              type="password"
              name="password"
              placeholder="**********"
              class="inputConnectSubscribe"
              v-model="user.password"
            />
          </div>
          <div>
            <!-- repeatPassword -->
            <label for="repeatPassword">Répéter le mot de passe</label><br />
            <input
              type="password"
              name="repeatPassword"
              placeholder="**********"
              class="inputConnectSubscribe"
              v-model="user.repeatPassword"
            />
          </div>
          <!-- <div id="select">
            <div>
              <label for="selectLanguage">langage maitrisé</label>
              <select
                name="selectLanguage"
                id="selectLanguage"
                v-model="user.selectLanguage"
              >
                <option value="Javascript">Javascript</option>
                <option value="PHP">PHP</option>
                <option value="Ruby">Ruby</option>
              </select>
            </div>
            <div>
              <label for="selectLevel">Niveau</label>
              <select
                name="selectLevel"
                id="selectLevel"
                v-model="user.selectLevel"
              >
                <option value="Junior">Junior</option>
                <option value="Confirmé">Confirmé</option>
                <option value="Expert">Expert</option>
              </select>
            </div>
            <button @click.prevent="pushLanguage">Ajout langage</button>
          </div>
          <div>
            <span v-for="item in languageUser" :key="item.selectLanguage">
              {{ item }}
            </span>
          </div> -->

          <button @click.prevent="pushUser" class="btnConnectSubscribe">
            S'inscrire
          </button>
          <p>
            Déja membre ?
            <button @click="subscribeToConnection" class="btnConnectSubscribe">
              Connectez-vous
            </button>
          </p>
        </div>
      </div>
    </div>
    <!-- NAVBAR -->
    <!-- NAVBAR -->
    <!-- NAVBAR -->
    <div id="navbar">
      <!-- BOUTON ACCUEIL -->
      <router-link to="/"
        ><font-awesome-icon id="faHome" icon="home"
      /></router-link>
      <!-- LOGO NAVBAR -->
      <div id="logos">
        <router-link to="/"><p id="logo">[LinkDev]</p></router-link>
      </div>

      <!-- BARRE DE RECHERCHE
      <input type="text" placeholder="Rechercher sur LinkDev " /> -->
      <!-- SI  isConnected==TRUE alors -->
      <div v-if="isConnected == false">
        <!-- Fait apparaitre la div connection -->
        <button @click="linkConnection" id="linkConnection">
          <span id="connectLink">Connection</span>
        </button>
        |
        <button @click="linkSubscribe" id="linkSubscribe">
          <span id="subscribeLink">Inscription</span>
        </button>
      </div>
      <!-- sinon -->
      <div v-else>
        <router-link to="/User"
          ><span> {{ user.pseudo }} </span></router-link
        >
        <button @click="logout" id="buttonLogout">
          <font-awesome-icon icon="power-off" />
        </button>
        <!-- <button @click="checkToken">CheckToken</button> -->
      </div>
    </div>
    <router-view />
  </div>
</template>

<script>
export default {
  data: () => ({
    tokenUser: "",
    // Connection
    logMail: "",
    logPassword: "",
    isConnected: false,
    divConnexion: false,
    // Inscription
    user: {
      pseudo: "",
      email: "",
      password: "",
      repeatPassword: "",
      selectLanguage: "",
      selectLevel: "",
    },
    languageUser: [],
    divSubscribe: false,
  }),

  /* Options de la requête */
  methods: {
    checkToken: async function() {
      const token = this.tokenUser;

      const options = {
        method: "GET", // Verbe
        headers: {
          Authorization: "bearer " + token,
        },
      };

      /* Tentative de requête */
      try {
        /* Envoi de la requête */
        const response = await fetch(
          "https://link-dev-api.osc-fr1.scalingo.io/user",
          options
        );

        console.log(response); // Réponse
        console.log(this.tokenUser);

        const data = await response.json(); // Lire la réponse au format JSON
        this.user.pseudo = data.name;
        this.user.email = data.email;
        console.log(data); // Body de la réponse
      } catch (error) {
        /* En cas d'erreur lors de l'exécutino de la requête */
        console.log(error);
      }
    },

    checkLogin: function() {
      return this.isConnected;
    },

    login: function() {
      this.isConnected = true;
    },
    logout: function() {
      this.isConnected = false;
    },

    // Connection
    validLogin: async function() {
      // FETCH CONNEXION
      const body = {
        email: this.logMail,
        password: this.logPassword,
      };

      const options = {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },

        body: JSON.stringify(body),
      };

      try {
        const response = await fetch(
          "https://link-dev-api.osc-fr1.scalingo.io/login",
          options
        );

        if (response.status != 200) {
          this.isConnected = false;
        } else {
          this.isConnected = true;
          this.divConnexion = false;
        }

        console.log(response);

        const data = await response.json();
        this.tokenUser = data.token;
        const checkToken = this.checkToken;
        checkToken();
        console.log(data);
      } catch (error) {
        console.log(error);
      }
    },

    linkConnection: function() {
      this.divConnexion = true;
    },

    closeConnexion: function() {
      this.divConnexion = false;
    },

    // Subscribe
    pushUser: async function() {
      /* Conditions Subscribe */
      if (this.user.password != this.user.repeatPassword) {
        alert("les mots de passe doivent être identiques");
        return;
      }
      if (this.user.pseudo == "") {
        alert("Entrez un pseudo");
        return;
      }
      if (this.user.email == "") {
        alert("Entrez un Email");
      }
      if (this.user.password == "") {
        alert("Entrez un mot de passe");
      }

      /* Envois requêtes Subscribe */
      const body = {
        name: this.user.pseudo,
        email: this.user.email,
        password: this.user.password,
      };
      const options = {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(body),
      };
      try {
        const response = await fetch(
          "https://link-dev-api.osc-fr1.scalingo.io/register",
          options
        );

        if (response.status == 200) {
          this.divSubscribe = false;
          this.divConnexion = true;
        }

        console.log(response);
        const data = await response.json();
        console.log(data.name);
      } catch (error) {
        console.log(error);
      }
    },
    pushLanguage: function() {
      this.languageUser.push(this.user.selectLanguage, this.user.selectLevel);
    },
    linkSubscribe: function() {
      this.divSubscribe = true;
    },
    closeSubscribe: function() {
      this.divSubscribe = false;
    },
    connectionToSubscribe: function() {
      this.divConnexion = false;
      this.divSubscribe = true;
    },
    subscribeToConnection: function() {
      this.divSubscribe = false;
      this.divConnexion = true;
    },
  },

  provide: function() {
    return {
      login: this.login,
      logout: this.logout,
      checkLogin: this.checkLogin,
      linkConnection: this.linkConnection,
      user: this.user,
    };
  },
};
</script>

<style>
#faHome {
  margin-left: 20px;
  font-size: 30px;
  padding: 10px;
  border-radius: 5px;
}
#faHome:hover {
  background-color: rgb(73, 73, 73);
}
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
  background-image: url(https://vogappdevelopers.com/wp-content/uploads/2019/04/vw-backv2.jpg);
  background-color: #cccccc;
  height: 100vh;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  background-attachment: fixed;
}
#navbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: #565656;
  height: 8vh;
}
#navbar input {
  height: 40px;
  width: 20%;
  border-radius: 10px;
}
#navbar a {
  text-decoration: none;
  color: white;
}
#logo {
  font-size: 35px;
  padding: 5px;
  color: white;
  margin: 0 15px;
  position: absolute;
  left: 94vh;
  top: -1px;
}
#logo:hover {
  background-color: rgb(73, 73, 73);
  border-radius: 5px;
}
#connectLink::before,
#subscribeLink::before {
  content: "< ";
}
#connectLink::after,
#subscribeLink::after {
  content: " / >";
}

#buttonLogout {
  border: none;
  margin: 0 20px;
  background-color: transparent;
  font-size: 30px;
  padding: 10px;
  border-radius: 5px;
  color: white;
}
#buttonLogout:hover {
  cursor: pointer;
  background-color: rgb(73, 73, 73);
}
#linkConnection {
  padding: 10px;
  border: none;
  background-color: transparent;
  color: white;
  font-size: 17px;
  cursor: pointer;
}
#linkConnection:hover,
#subscribeLink:hover {
  padding: 10px;
  background-color: rgb(73, 73, 73);
  border-radius: 5px;
}
#subscribeLink {
  cursor: pointer;
  padding: 10px;
}
#boxConnect {
  background-color: white;
  box-shadow: 0 0 8px 1px grey;
  padding: 20px;
  border-radius: 5px;
  width: 91%;
  margin: auto;
  height: 58vh;
}
.btnHome {
  border: 1px solid black;
  color: black;
  text-decoration: none;
}
#fondGris {
  width: -webkit-fill-available;
  height: 100vh;
  position: absolute;
  background-color: #7d7b7be0;
}

#divConnexion {
  height: 57vh;
  width: 63vh;
  position: fixed;
  left: 36%;
  top: 18%;
}
#buttonLeaveConnection,
#buttonLeaveSubscribe {
  font-size: 15px;
  padding: auto;
  border: none;
  margin: auto;
  float: right;
  background-color: transparent;
}
#buttonLeaveConnection:hover,
#buttonLeaveSubscribe:hover {
  cursor: pointer;
  margin: auto;
  padding: auto;
  border-radius: 5px;
  background-color: gray;
  color: white;
}
/* Subscribe */
#subscribe {
  background-color: white;
  box-shadow: 0 0 8px 1px grey;
  padding: 20px;
  border-radius: 5px;
  width: 91%;
  margin: auto;
  height: 58vh;
}
#nameSurname {
  display: flex;
}
#select {
  display: flex;
  justify-content: center;
}

#divSubscribe {
  height: 57vh;
  width: 63vh;
  position: fixed;
  left: 36%;
  top: 18%;
}
#linkSubscribe {
  border: none;
  background-color: transparent;
  color: white;
  font-size: 17px;
}
/* Style Input connexion */
.inputConnectSubscribe {
  border: none;
  border-bottom: 2px solid grey;
  margin: 10px;
  padding: 5px;
}
.btnConnectSubscribe {
  background-color: transparent;
  color: grey;
  border: none;
  border: 2px solid grey;
  border-radius: 4px;
  cursor: pointer;
}
.btnConnectSubscribe:hover {
  color: white;
  background-color: grey;
  box-shadow: 0 0 6px 1px grey;
}
/* test */

/* .box::before,
.box::after {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  background: linear-gradient(
    45deg,
    #ffffff,
    #ffffff,
    #ffffff,
    #000000,
    #000000,
    #000000,
    #ffffff,
    #ffffff,
    #ffffff
  );
  width: 100%;
  height: 100%;
  transform: scale(1.02);
  z-index: -1;
  background-size: 500%;
  animation: animate 20s infinite;
  border-radius: 10px;
}

.box::after {
  filter: blur(20px);
}

@keyframes animate {
  0% {
    background-position: 0 0;
  }
  50% {
    background-position: 300% 0;
  }
  100% {
    background-position: 0 0;
  }
} */
</style>
