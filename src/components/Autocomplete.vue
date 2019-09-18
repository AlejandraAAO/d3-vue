<template>
  <div class="autocomplete">
    <input type="text" 
    v-model="search" 
    @input="onChange" 
    @keydown.down="onArrowDown"
    @keydown.up="onArrowUp"
    @keydown.enter="onEnter"
    />
    <ul id="autocomplete-results" 
    class="autocomplete-results" 
    v-show="isOpen">
      <li class="loading" v-if="isLoading">
        Cargando resultados ...
      </li>
      <li 
      v-else 
      v-for="(result, i) in results" 
      :key="i" 
      @click="setResult(result)"
      class="autocomplete-result"
      :class="{'is-active': i === arrowCounter}">
        {{result}}
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  name: "autocomplete",
  props: {
    items: {
      type: Array,
      required: false,
      default: () => []
    },
    isAsync:{
      type:Boolean,
      required:false,
      default:false
    }
  },
  data() {
    return {
      search: '',
      results: [],
      isOpen: false,
      isLoading: false,
      arrowCounter:0
    };
  },
  methods: {
    onArrowDown(){
      if(this.arrowCounter < this.results.length){
        this.arrowCounter = this.arrowCounter + 1
      }
    },
    onArrowUp(){
      if(this.arrowCounter > 0){
        this.arrowCounter = this.arrowCounter -1
      }
    },
    onEnter(){
      this.search = this.results[this.arrowCounter];
      this.isOpen = false;
      this.arrowCounter = -1;
    },
    onChange() {
      this.$emit('input',this.search);

      if(this.isAsync){
        this.isLoading = true;
      } else {
        this.filterResults();
        this.isOpen = true;
      }
      
    },
    filterResults() {
      this.results = this.items.filter(
        item => item.toLowerCase().indexOf(this.search.toLowerCase()) > -1
      );
    },
    setResult(result){
      this.search = result;
      this.isOpen = false;
    },
    handleClickOutside(evt){
      if(!this.$el.contains(evt.target)){
        this.isOpen = false;
        this.arrowCounter = -1
      }
    }
  },
  watch:{
    items: function(val,oldValue){
      if(val.length !== oldValue.length){
        this.results = val;
       /*  this.isOpen = true; */
        this.isLoading = false;
      }
    }
  },
  mounted(){
    document.addEventListener('click',this.handleClickOutside);
  },
  destroyed(){
    document.removeEventListener('click',this.handleClickOutside);
  }
};
</script>
<style>
.autocomplete {
  position: relative;
  width: 130px;
}

.autocomplete-results {
  padding: 0;
  margin: 0;
  border: 1px solid #eeeeee;
  height: 120px;
  overflow: auto;
}

.autocomplete-result {
  list-style: none;
  text-align: left;
  padding: 4px 2px;
  cursor: pointer;
}

.autocomplete-result:hover {
  background-color: #4aae9b;
  color: white;
}
.autocomplete-result.is-active,
.autocomplete-result:hover {
  background-color: #4AAE9B;
  color: white;
}
</style>