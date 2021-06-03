<template>
  <div class="about">
    <br />
    <div style="overflow: scroll;overflow-x: hidden;" id="user">
      <br />
      <div
        style="box-shadow : 0 0 8px 1px grey ;width:800px; margin:auto; border-radius: 5px"
        id="profilCard"
      >
        <div id="pictureHeader">
          <img
            style="border-top-left-radius : 5px; border-top-right-radius :5px"
            width="800px"
            height="200px"
            src="https://pic.clubic.com/v1/images/1709696/raw?width=1200&fit=max&hash=b66a110eab5bfb759d547630534eb1be823a7b82"
            alt=""
          />
        </div>
        <div style=" display:flex;" id="profilContent">
          <div style="width:20%;border-right: 1px solid grey">
            <img
              style="border-radius:50%;margin : 12px;"
              height="70px"
              src="https://www.pngitem.com/pimgs/m/146-1468479_my-profile-icon-blank-profile-picture-circle-hd.png"
            />
          </div>
          <div style="text-align:justify ; padding : 10px">
            <p style="font-weight : bold; font-size : 20px">
              {{ user.pseudo }}
            </p>
            <p style="color : grey; font-style : italic">
              Développeur Full Stack | Entreprise : {{ entreprise }}
            </p>
            <span
              style="color : grey; font-style : italic ;font-size : 11px;display:flex; justify-content : space-between"
            >
              <p style="display : inline-block">
                Région PACA - Nice Côte d'Azur
              </p>
            </span>
          </div>
        </div>
        <br />
        <!-- Btn + conditions - informations & contact -->

        <div style="text-align : justify; padding : 10px">
          <div
            style="display:flex; justify-content: space-around;margin-bottom:20px"
          >
            <button style="margin-left:40px; width:150px" @click="btnInfo">
              Languages maitrisé
            </button>
            <button style="width:110px" @click="btnContact">
              Me contacter
            </button>
            <span v-if="checkLogin() === true"
              ><button @click="btnModif" style="margin-right:40px;width:150px">
                Modifier le profil
              </button></span
            >
          </div>

          <!-- Condition et tableau langage-->
          <div class="boxInfo" v-if="tableLangage === false">
            <table style="margin:auto">
              <tr>
                <th class="modifTh">Language</th>
                <th>Niveau de compétence</th>
              </tr>
              <tr>
                <td class="modifTh">JaveScript</td>
                <td>Expert</td>
              </tr>
              <tr>
                <td class="modifTh">PHP</td>
                <td>Intermédiaire</td>
              </tr>
            </table>
          </div>
          <!-- Condition et tableau Contact-->
          <div class="boxInfo" v-if="tableContact === false">
            <p>Email : {{ user.email }}</p>
            <p>Numéro de téléphone : 0123456</p>
          </div>
          <!-- Condition et tableau Modification de profil-->
          <div v-if="checkLogin() === true">
            <div
              style="display:flex;justify-content: space-around;text-align:center"
              class="boxInfo"
              v-if="tableModif === false"
            >
              <div>
                <!-- Message d'excuse -->
                <span style="color:grey; font-size:10px"
                  >*Vous devez remplir
                  <span
                    style="font-style:italic;font-weight:bold ;color:red; text-decoration:underline"
                    >tous les champs</span
                  >
                  pour modifier vos données. Un champs vide retirera la valeur
                  de votre information précédente.<br />Navré pour la gêne
                  occasionnée, l'équipe LinkDev</span
                ><br /><br />
                <!-- Modification Pseudo -->
                <label for="modifPseudo">Pseudo :</label>
                <input
                  placeholder="New Pseudo"
                  style="margin-right:30px"
                  id="modifPseudo"
                  v-model="inputPseudo"
                  type="text"
                />
                <!-- Modification Entreprise -->
                <label style="margin-left:30px" for="modifEmail"
                  >Entreprise :</label
                >
                <input
                  placeholder="Votre Entreprise"
                  v-model="inputEntreprise"
                  type="text"
                /><br /><br />
                <button @click="validModif" style="margin-auto">Valider</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  inject: ["user", "logout", "login", "checkLogin", "linkConnection"],

  data() {
    return {
      tableLangage: true,
      tableContact: true,
      tableModif: true,
      inputPseudo: "",
      inputEntreprise: this.entreprise,
      entreprise: "Le Bocal",
    };
  },
  methods: {
    btnInfo: function() {
      this.tableLangage = !this.tableLangage;
      this.tableContact = true;
      this.tableModif = true;
    },
    btnContact: function() {
      this.tableContact = !this.tableContact;
      this.tableLangage = true;
      this.tableModif = true;
    },
    btnModif: function() {
      this.tableModif = !this.tableModif;
      this.tableLangage = true;
      this.tableContact = true;
    },

    validModif: function() {
      this.user.pseudo = this.inputPseudo;
      this.entreprise = this.inputEntreprise;
      this.inputEntreprise = "";
      this.inputPseudo = "";
    },
  },
};
</script>
<style scoped>
#user {
  box-shadow: 0 0 5px 1px white;
  border-radius: 5px;
  margin: auto;
  width: 75%;
  background-color: white;
  height: 89vh;
}
#userProfile {
  height: 35vh;
  width: 60%;
  margin: auto;
  background-color: burlywood;
}

button {
  font-weight: 540;
  padding: 5px;
  font-size: 15px;
  background-color: transparent;
  color: grey;
  border: none;
  box-shadow: 0 0 2px 1px grey;
  border-radius: 6px;
  cursor: pointer;
}
button:hover {
  color: white;
  background-color: grey;
  box-shadow: 0 0 6px 1px grey;
}

.boxInfo {
  border-radius: 2px;
  margin: 5px;
  padding: 10px;
  background-color: #f7f7f7;
  box-shadow: 0 0 2px 1px grey;
}
input {
  border: none;
  font-style: italic;
  color: grey;
  border-bottom: 2px solid grey;
  margin: 10px;
  padding: 2px;
  background-color: transparent;
}
.modifTh {
  padding-right: 50px;
}
th,
td {
  padding-bottom: 10px;
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
</style>
