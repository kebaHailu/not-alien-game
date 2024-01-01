<template>
   <div  id = "app" class="contain">


    <GameStateStart v-if="uiState === 'start' " > 
      <h3>Which human do you want to be?</h3>
      <p 
      class="choice" 
      v-for="option in characterChoices" 
      :key="option">

      <input type="radio" v-model="characterinput" :id="option" :value="option" />
      <label :for="option" > {{ option }}</label>

      </p>
      <button @click="startGame">Start Game</button>

    </GameStateStart>

    <section v-else-if="uiState === 'playing'">
      <svg viewBox="0 -180 1628 1180" class="main">
        <defs>
          <clipPath id="bottom-clip">
            <rect class="bottom-clip-path" x="1131.5" y="546.5" width="406" height="1000" />
          </clipPath>
          <clipPath id="top-clip">
            <rect class="top-clip-path" x="1131.5" y="69.5" width="406" height="473" />
          </clipPath>
        </defs>
        
        <Friend/>
        <score/>
        
        <component :is="character" >
        
        </component>

        <text
          x="1000"
          y="930"
          style="font: normal 45px 'Recursive; text-transform: uppercase;"
          class="text"
        >  {{ character }}  </text>

        <path fill="#f0959f" d="M0 842h657v192H0z" />

        <g id="friendbubble">
          <path
            class="cls-45"
            d="M342.3 471.8h106.6c0 42.6-21.3 63.9-21.3 63.9 64 0 85.3-63.9 85.3-63.9h42.6c42.7 0 42.7-42.7 42.7-42.7s21.3-149.3 21.3-191.9-42.6-42.7-42.6-42.7H321c-42.7 0-42.7 42.7-42.7 42.7l21.4 191.9s0 42.7 42.6 42.7z"
            transform="translate(17)"
          />
          <path
            class="cls-20"
            d="M333.8 463.2h106.6c0 42.7-21.3 64-21.3 64 63.9 0 85.3-64 85.3-64H547c42.7 0 42.7-42.6 42.7-42.6s21.3-149.3 21.3-192-42.7-42.6-42.7-42.6H312.4c-42.6 0-42.6 42.6-42.6 42.6l21.3 192s0 42.6 42.7 42.6z"
            transform="translate(17)"
          />
        </g>

        <g id="alienbubble">
          <path
            class="cls-45"
            d="M948.3 344.5c-103.8 0-187.9 76.3-187.9 170.4s84.1 170.3 187.9 170.3a201.5 201.5 0 00100.5-26.4l87.4 26.4-29.1-79.2c18.4-26.4 29.1-57.6 29.1-91.1 0-94.1-84.1-170.4-187.9-170.4z"
            transform="translate(17)"
          />
          <path
            class="cls-20"
            d="M938.9 336C835.1 336 751 412.3 751 506.4s84.1 170.3 187.9 170.3a201.5 201.5 0 00100.5-26.4l87.4 26.4-29.1-79.2c18.4-26.4 29.1-57.6 29.1-91.1 0-94.1-84.1-170.4-187.9-170.4z"
            transform="translate(17)"
          />
        </g>

      </svg>

      <div class="friendtalk">
          <h3>{{ questions[questionIndex].question  }}</h3>
      </div>
      <div class="zombietalk">
        <p v-for="character in shuffle(characterChoices)">
          <button @click="pickQuestion(character)"> {{questions[questionIndex][character]}} </button>
        </p>
      </div>
    </section>

    <GameStateFinish v-else-if="uiState === 'lost' " > lost 
    </GameStateFinish>
    <GameStateFinish v-else-if="uiState === 'won' " > won 
    </GameStateFinish>
  </div>
</template>

<script>
import { mapState } from 'vuex';
import GameStateStart from './components/GameStateStart.vue';
import Friend from './components/Friend.vue';
import score from './components/score.vue';
import Artist from './components/Artist.vue';
import Baker from './components/Baker.vue';
import Mechanic from './components/Mechanic.vue';
import zombie from './components/zombie.vue';
import GameStateFinish from './components/GameStateFinish.vue';




export default {
  name: 'App',
  components: {
    GameStateStart,
    GameStateFinish,
    Friend,
    score,
    Artist,
    Baker,
    Mechanic,
    zombie,
  },
  data () {
    return {
      characterinput:''
    }
  },
  computed: {
    ...mapState([
      'uiState',
      'questions',
      'characterChoices',
      'character',
      'questionIndex',
      'score',

  ]),
  },
  methods:{
    startGame(){
      this.$store.commit('pickCharacter', this.characterinput);
      this.$store.commit('updateUiState', 'playing');
    },
    pickQuestion(char){
      this.$store.commit('pickQuestion', char);
      this.$store.commit('updateIndex')
    },
    shuffle(array){
      for ( let i = array.length - 1; i>0; i--){
        const j = Math.floor(Math.random() * (i+1))
        ;[array[i],array[j]] = [array[j],array[i]]
      }
      return array 
    }
  }
};

</script>
