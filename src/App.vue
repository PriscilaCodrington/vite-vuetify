<script>
import axios from "axios";

export default {
  data() {
    return {
      currentDogLink:
        "https://images.dog.ceo/breeds/chihuahua/n02085620_3407.jpg",
      favoriteDogs: [],
    };
  },
  methods: {
    loadNewDog() {
      axios
        .get("https://dog.ceo/api/breeds/image/random")
        .then((response) => {
          this.currentDogLink = response.data.message;
        })
        .catch((error) => {
          console.log(error);
        });
    },

    addToFavorites() {
      this.favoriteDogs.push(this.currentDogLink);
    },
    removeToFavorites(index) {
      this.favoriteDogs.splice(index, 1);
    },
  },
  created() {
    this.loadNewDog();
  },
  computed: {
    isAlreadyInFavorites() {
      return this.favoriteDogs.indexOf(this.currentDogLink) > -1;
    },
  },
};
</script>

<template>
  <v-app>
    <v-main class="dogs-layout">
      <v-container fill-height>
        <div class="dogs-overlay">
          <h1 class="display-2 text-xs-center">Choose your favorite dogs</h1>
          <v-card class="dog-card">
            <v-img height="400px" :src="currentDogLink"></v-img>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                icon
                @click="addToFavorites"
                :disabled="isAlreadyInFavorites"
              >
                <span class="material-icons">favorite</span>
              </v-btn>
              <v-btn icon @click="loadNewDog">
                <span class="material-icons">forward</span>
              </v-btn>
            </v-card-actions>
            <v-container grid-list-md fluid>
              <v-row>
                <v-col cols="3" v-for="(pet, index) in favoriteDogs">
                  <v-card class="dog-card">
                    <v-img height="150px" :src="pet" />
                    <v-card-actions>
                      <v-spacer /><!--Good practice to autoclose because it doesnt have something inside-->
                      <v-btn icon @click="removeToFavorites(index)">
                        <span class="material-icons">delete</span>
                      </v-btn>
                    </v-card-actions>
                  </v-card>
                </v-col>
              </v-row>
            </v-container>
          </v-card>
        </div>
      </v-container>
    </v-main>
  </v-app>
</template>

<style>
img {
  max-width: 100%;
}

h1 {
  padding-bottom: 15px;
}

.dogs-layout {
  width: 100%;
  background: #fff
    url("https://github.com/FrontEndFoxes/projects/blob/main/petshop/images/bg3.jpg?raw=true")
    repeat center;
}

.dogs-overlay {
  width: 100%;
  padding: 20px;
  background-color: #fff;
  display: flex;
  flex-direction: column;
  align-items: center;
}

@media (max-width: 768px) {
  .dogs-overlay {
    margin: 0;
  }
}

.dog-card {
  width: 100%;
  max-width: 600px;
}

.material-icons {
  font-family: "Material Icons";
  font-weight: normal;
  font-style: normal;
  font-size: 24px; /* Preferred icon size */
  display: inline-block;
  line-height: 1;
  text-transform: none;
  letter-spacing: normal;
  word-wrap: normal;
  white-space: nowrap;
  direction: ltr;

  /* Support for all WebKit browsers. */
  -webkit-font-smoothing: antialiased;
  /* Support for Safari and Chrome. */
  text-rendering: optimizeLegibility;

  /* Support for Firefox. */
  -moz-osx-font-smoothing: grayscale;

  /* Support for IE. */
  font-feature-settings: "liga";
}
</style>
