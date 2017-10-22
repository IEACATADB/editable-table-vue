<template>
  <tr>
    <Cell
      v-for="cell,index in cells" 
      :key="index"
      :column="index"
      :content="cell"
      :row="row" 
      :selected="selected_row && index == selected_column"
     />
  </tr>
</template>

<script>
import Cell from './Cell'
export default {
  name: 'Row',
  props:{
    cells:{
        type:Array ,
        required:true
     },
     row: Number,
     selected_row: Boolean,
     selected_column: Number,
     bus:{}
  },
  components:{
      Cell
  }
}
</script>
