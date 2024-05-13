<script lang="ts">
import { onMounted, reactive, ref, watchEffect } from "vue";
import Results from "./Results.vue"
export default {
  components:{
    Results
  },
  setup() {
   
  
    const data = ref([
      "morning", "sun", "cast", "warm", "glow", "over", "the", "quiet", "village", "the", "inhabitants", "began", "stir", "from", "their", "slumber",
      "Birds", "chirped", "melodiously", "the", "trees", "adding", "the", "serene", "atmosphere", "that", "enveloped", "the", "quaint", "streets",
      "The", "aroma", "freshly", "brewed", "coffee", "wafted", "through", "the", "air", "mingling", "with", "the", "scent", "blooming", "flowers",
      "the", "town", "square", "the", "marketplace", "bustled", "with", "activity", "vendors", "set", "up", "their", "stalls", "displaying", "array", "colorful", "fruits", "vegetables", "and", "handmade", "crafts",
      "Locals", "and", "visitors", "alike", "meandered", "through", "the", "bustling", "stalls", "exchanging", "greetings", "and", "laughter", "they", "perused", "the", "goods", "offer",
      "Meanwhile", "children", "skipped", "along", "the", "cobblestone", "streets", "their", "laughter", "echoing", "off", "the", "ancient", "buildings", "that", "lined", "the", "thoroughfare",
      "Some", "played", "hopscotch", "while", "others", "chased", "each", "other", "game", "tag", "their", "carefree", "spirits", "adding", "sense", "joy", "the", "morning", "air",
      "As", "the", "day", "progressed,", "the", "sun", "reached", "its", "zenith,", "casting", "a", "golden", "hue", "over", "the", "landscape.",
      "The", "villagers", "gathered", "in", "the", "shade", "of", "the", "town", "square,", "seeking", "relief", "from", "the", "intense", "heat.",
      "Children", "splashed", "in", "the", "cool", "waters", "of", "the", "nearby", "stream,", "their", "laughter", "filling", "the", "air", "with", "joy.",
      "In", "the", "fields", "surrounding", "the", "village,", "farmers", "toiled", "under", "the", "midday", "sun,", "tending", "to", "their", "crops",
      "The", "smell", "of", "freshly", "turned", "earth", "mingled", "with", "the", "sweet", "scent", "of", "ripening", "fruit.",
      "As", "evening", "approached,", "the", "village", "came", "alive", "with", "the", "sounds", "of", "preparation", "for", "the", "night's", "festivities.",
      "Bonfires", "were", "lit,", "and", "tables", "were", "set", "with", "an", "abundance", "of", "food", "and", "drink.",
      "The", "sounds", "of", "music", "and", "laughter", "drifted", "through", "the", "streets", "as", "the", "celebration", "began."
    ]);
    
    function shuffle(array: string[]) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
      return array;
    }
    const box = ref()

    const words = ref(shuffle(data.value));
    const bg = ref();
    const bgArr = ref(new Array(words.value.length).fill(null));
    const width = ref()
    const obj = reactive({
      time: 60,
      index: 0,
      currWord: words.value[0],
      correctWords: 0,
      wrongWords: 0,
      search:"",
      resultPop:false,
      wpm:0,
      perMinute:0
    });
    const firstKey = ref(false);
    onMounted( function(){
        width.value = document.querySelectorAll('.wordContainer');
        




      
    })
    
watchEffect(async() => {
  if(obj.search[obj.search.length-1]===" "){
    obj.search=obj.search.slice(0,obj.search.length-1)
     keySpace()
    obj.search=""
    return;
  }
  else if(obj.search==obj.currWord.slice(0,obj.search.length)){
    bg.value=true
  }
  else{
    bg.value=false
  }

  if(obj.search.length==0){
    bg.value=null
  }
  
})
  
    
    
    




    const keySpace=()=>{
      if(obj.search===obj.currWord){
        bgArr.value[obj.index] = true;
        bg.value = null;
        obj.correctWords += 1;
        obj.wpm+=obj.search.length
        
        
      
      }
      else{
      bgArr.value[obj.index] = false;
        bg.value = null;
        obj.wrongWords += 1;
  }
      obj.index += 1;
      obj.currWord = words.value[obj.index];
    if (width.value[obj.index].offsetTop !==width.value[obj.index-1].offsetTop ) {
        words.value = words.value.slice(obj.index);
        obj.index = 0;
    }
      

  return;
    }
    
  
  


    
    const timeStart = function (): void {
      let interval = setInterval(function () {
        obj.time = obj.time - 1;
        obj.perMinute=(15*(obj.wpm)/(60-obj.time)).toFixed(2)
        if (obj.time === 0) {
          obj.resultPop=true
          clearInterval(interval);
        }
      }, 1000);
    };

    const keyStart = function () {

      timeStart();
      firstKey.value = true;
    };

   
    return { words, obj, keyStart, firstKey, keySpace, bg, bgArr,box };
  }
};
</script>

<template>
  <div class="boxofwords">
  
    <div class="allwords">
      <div class="wordContainer" v-for="(word, index) in words" :key="index">
        <p class="word"
          ref="box"
          :class="{ 
            active: obj.index === index,
            pastBg: bgArr[index] === true && obj.index > index,
            pastBgFalse: bgArr[index] === false && obj.index > index,
            bg:obj.index==index&& bg==true,
            bgfalse: obj.index==index&&bg==false,
          }">
          {{ word }}
        </p>
      </div>
    </div>
    <div class="searchInput"> 
      <input 
        :disabled="obj.resultPop"
         autocomplete="off"
        type="text" 
        name="search" 
        id="search" 
        @keyup="firstKey === false && keyStart()"
        v-model="obj.search" 
        placeholder="Enter"
      >      
          <div class="wpm">
        {{obj.perMinute}} WPM

      </div>
      <div class="timeContainer">
        {{ obj.time }}
      </div>
  
    </div>
      <div class="results" v-if="obj.resultPop">
  <Results :correctWords="obj.correctWords" :wrongWords="obj.wrongWords" :wpm="obj.wpm/4"/>
      </div>
  </div>
  
</template>

<style scoped>
.boxofwords {
  flex-direction: column;
  height: 100%;
  width: 100%;
  align-items: center;
  justify-content: center;
  display: flex;
  
}

.boxofwords .allwords {
  background-color: #fff;
  width: 1200px;
  height: 180px;
  overflow-x: hidden;
  overflow-y: hidden;
  display: flex;
  flex-wrap: wrap;
  border-radius: 15px;
  padding: 5px 10px;
}

.boxofwords .allwords .wordContainer {
  padding: 3px 4px;
  height: 75px;
}

.boxofwords .allwords .wordContainer .word {
  font-weight: 500;
  padding: 4px;
  font-size: 29px;
}

.searchInput {
  width: 600px;
  margin-top: 25px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
}

#search {
  width: 85%;
  height: 90%;
  padding: 15px;
  font-size: 21px;
  border: 3px solid #cb00eabe;
  outline: none;
  box-shadow: rgba(50, 50, 93, 0.25) 0px 6px 12px -2px, rgba(0, 0, 0, 0.3) 0px 3px 7px -3px;
}

.timeContainer {
  width: 15%;
  background: #fff;
  border: 3px solid #cb00eabe;

  height: 70px;
  color: #000000;
  font-size: 21px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-left: 10px;
  box-shadow: rgba(50, 50, 93, 0.25) 0px 6px 12px -2px, rgba(0, 0, 0, 0.3) 0px 3px 7px -3px;
}

.active {
  background: rgb(197, 197, 197);
}


.bg,.pastBg {
  background: rgb(113, 226, 113);
  border-radius: 5px;
}

.bgfalse,.pastBgFalse{
  background: rgb(232, 101, 101);
}
.results{
  margin-top: 50px;
  display: flex;
  align-items: flex-start;
  justify-content: start;
  text-align: left;
  background: #fff;
  padding: 30px;
  font-size: 25px;
  
}

.wpm{
  width: 25%;
   background: #fff;
  border: 3px solid #cb00eabe;
  height: 70px;
  font-size: 21px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-left: 10px;
  box-shadow: rgba(50, 50, 93, 0.25) 0px 6px 12px -2px, rgba(0, 0, 0, 0.3) 0px 3px 7px -3px;

}
</style>
