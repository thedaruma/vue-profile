<template lang="html">
<div class="sub-menu">
<p>Select a character to view their stats.</p>
    <transition-group @enter="fadeInUp">
      <character-select @click.native="viewDetails(character)" v-for="(character,index) in characters":key="index" :character="character" ></character-select></transition-group>
    
</div>
</template>

<script>
import CharacterSelect from './CharacterSelect.vue';
import CharacterDetail from './CharacterDetail.vue';
import {AnimationMixin} from '../../../mixins/Animations';
export default {
  mixins:[AnimationMixin],
  props:['characters'],
  data:function(){
    return{
      selectedCharacter:null,
      duration:.3
      // characters:this.characters
    }
  },
  components:{
    characterSelect:CharacterSelect,
    characterDetail:CharacterDetail
  },
  created(){
    this.$emit('pageLoad',{stateText:'Learn more about our respective skills.'})
  },
  methods:{
    viewDetails(e){
      this.$router.push({path:`/status/detail/${e.id}`})
    },

  }
}
</script>

<style scoped>

</style>
