<template>
  <td @dblclick.stop.prevent="select" >

      <input v-show="selected" 
      @keyup.enter.stop.prevent ="update"
      @blur.stop="update"
      v-model="input_content" 
      size=1
      />
      <span v-show="!selected">
        {{content}}
      </span>
   
  </td>
</template>

<script>
import { EventBus } from './EventBus.js'
export default {
  name: 'Cell',
  props:{
    selected:{
      type:Boolean,
      default:false
      },
    column:Number,
    row:Number,
    content:[Number,String]
  },
  data(){
    return{
      input_content: this.content 
    }
  },
  watch:{
    content:function(){
      this.input_content = this.content;
    }
  },
  methods:{
    select: function(){
      EventBus.$emit('select', this.row, this.column)
    },
    update: function(){
      EventBus.$emit('update',this.input_content, this.row, this.column )
    }
  }
  
}
</script>

