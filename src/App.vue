<template>
  <div id="inicio">
    <div
      class="
        min-h-screen
        w-full
        h-full
        bg-no-repeat bg-cover
        items-center
        justify-center
      "
      style="
        background-image: url('https://fiverr-res.cloudinary.com/images/q_auto,f_auto/gigs/170508638/original/7c9a6119382e535e34d5e42cd412fb5f3c79e552/create-pixel-art-background-for-your-game.png');
      "
    >
      <div class="grid grid-cols3 gap-4 grid-flow-col flex justify-center">
        <input
          v-on:click="atras"
          type="button"
          value="Prev"
          class="
            m-8
            px-5
            py-3
            border border-transparent
            text-base
            font-medium
            rounded-md
            text-indigo-600
            bg-white
            hover:bg-indigo-50
          "
        />
        <div>
          <input
            v-on:click="inicio"
            class="w-30 h-20"
            type="image"
            src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/98/International_Pok%C3%A9mon_logo.svg/1200px-International_Pok%C3%A9mon_logo.svg.png"
          />
        </div>
        <div class="flex space-x-4">
          <input
            v-on:click="adelante"
            type="button"
            value="Netx"
            class="
              m-8
              px-5
              py-3
              border border-transparent
              text-base
              font-medium
              rounded-md
              text-indigo-600
              bg-white
              hover:bg-indigo-50
            "
          />
        </div>
      </div>
      <div class="flex justify-between grid - grid-cols-5 gap-4 ">
        <!--Puchamones en cajas V-for -->
        <div
          v-for="(data, index) in pokemons"
          :key="index"
          class="
            
            max-w-xs
            w-screen
            rounded
            border-2 border-gray-900
            overflow-auto
            
          "
        >
        <div class="">
          <img
            :src="`https://play.pokemonshowdown.com/sprites/xyani/${data.nombre}.gif`"
            class="poke-size mx-auto "
          />
        </div>
          
          <div class="bg-gray-100 px3 py-2">
            <div class="font-bold text-base mb-1 text-indigo-600">
              Nombre: {{ data.nombre }}
            </div>
            <p class="text-gray-700 text-base text-indigo-600">
              Habilidad: {{ data.habi }}
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script >
import axios from "axios";
export default {
  name: "inicio",
  data() {
    return {
      checked: null,

      //////
      url: `https://pokeapi.co/api/v2/pokemon/?offset=0&limit=20`,
      next: null,
      previo: null,
      pokemons: [],
      URLTodos: [],
    };
  },
  mounted() {
    axios.get(this.url).then((response) => {
      const NextURL = response.data.next;
      this.next = NextURL;
      console.log(NextURL);
      const PrevURL = response.data.previous;
      this.previo = PrevURL;
      console.log(PrevURL);
      const PokeURL = response.data.results;
      this.URLTodos = PokeURL;
      console.log(PokeURL);

      for (let i = 0; i < 20; i++) {
        axios.get(`${this.URLTodos[i].url}`).then((response) => {
          let ejemplo = {
            imagen: response.data.sprites.front_default,
            nombre: response.data.name,
            habi: response.data.abilities[0].ability.name,
          };
          this.pokemons.push(ejemplo);
        });
      }
    });
  },
  methods: {
    adelante: function () {
      this.URLTodos = [];
      this.pokemons = [];
      axios.get(this.next).then((response) => {
        const NextURL = response.data.next;
        this.next = NextURL;
        console.log(NextURL);
        const PrevURL = response.data.previous;
        this.previo = PrevURL;
        console.log(PrevURL);
        const PokeURL = response.data.results;
        this.URLTodos = PokeURL;
        console.log(PokeURL);

        for (let i = 0; i < 20; i++) {
          axios.get(`${this.URLTodos[i].url}`).then((response) => {
            let ejemplo = {
              imagen: response.data.sprites.front_default,
              nombre: response.data.name,
              habi: response.data.abilities[0].ability.name,
            };
            this.pokemons.push(ejemplo);
          });
        }
      });
    },
    inicio: function () {
      this.URLTodos = [];
      this.pokemons = [];
      axios.get(this.url).then((response) => {
        const NextURL = response.data.next;
        this.next = NextURL;
        console.log(NextURL);
        const PrevURL = response.data.previous;
        this.previo = PrevURL;
        console.log(PrevURL);
        const PokeURL = response.data.results;
        this.URLTodos = PokeURL;
        console.log(PokeURL);

        for (let i = 0; i < 20; i++) {
          axios.get(`${this.URLTodos[i].url}`).then((response) => {
            let ejemplo = {
              imagen: response.data.sprites.front_default,
              nombre: response.data.name,
              habi: response.data.abilities[0].ability.name,
            };
            this.pokemons.push(ejemplo);
          });
        }
      });
    },
    atras: function () {
      if (this.previo == null) {
      } else {
        this.URLTodos = [];
        this.pokemons = [];
        axios.get(this.previo).then((response) => {
          const NextURL = response.data.next;
          this.next = NextURL;
          console.log(NextURL);
          const PrevURL = response.data.previous;
          this.previo = PrevURL;
          console.log(PrevURL);
          const PokeURL = response.data.results;
          this.URLTodos = PokeURL;
          console.log(PokeURL);

          for (let i = 0; i < 20; i++) {
            axios.get(`${this.URLTodos[i].url}`).then((response) => {
              let ejemplo = {
                imagen: response.data.sprites.front_default,
                nombre: response.data.name,
                habi: response.data.abilities[0].ability.name,
              };
              this.pokemons.push(ejemplo);
            });
          }
        });
      }
    },
  },
};
</script>
  

<style>
.poke-size {
  width: 100px;
  height: 100px;
}
</style>
