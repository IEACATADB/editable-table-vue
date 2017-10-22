<template>
  <div >
    <table class="table table-bordered" :class="meta">
      <thead>
        <Controls :length="fields.length" />
        <tr class="headers">
          <th v-for="header in fields">
            {{header}}
          </th>
        </tr>
      </thead>
       <tbody>
          <Row v-for="row,index in grid" 
            :key="index" 
            :cells="row"
            :row="index"
            :selected_row="index == selected_row"
            :selected_column="selected_column"
            
              />
        </tbody>
    </table>

  </div>
</template>

<script>
import { EventBus } from "./EventBus.js"
import Row from "./Row";
import Controls from "./Controls";

export default {
  name: "ETable",
  components: {
    Row,
    Controls
  },
  props: {
    fields: {
      type: Array,
      required: true
      },
    rows: {
      type: Number,
      required: true
    },
    meta: {
      type:String,
      required:false
    }
  },
  data() {
    return {
      grid: [],
      selected_row: -1,
      selected_column: -1
    };
  },
  created() {
    this.grid = this.emptyGrid();
    EventBus.$on('select',  (row,column) =>{
      this.selected_row = row;
      this.selected_column = column;
    })
    
    EventBus.$on('update', (content,row,column) =>{     
      this.grid[row][column] = content
      this.deselect()
    })

    EventBus.$on('add_row',(row_after_index) =>{
      this.add_row(row_after_index);
    })

    EventBus.$on('clear_table',() =>{
      this.clean_table();
    })

    EventBus.$on('get_data',() =>{
      console.dir(this.get_data());
    })
    
  },
  methods: {
    emptyGrid: function(height, width) {
      height = height || this.rows;
      width = width || this.fields.length;
      return this.emptyRow(height).map(() => this.emptyRow(width));
    },

    emptyRow: function(length) {
      length = length || this.fields.length;
      return Array(length).fill("");
    },

    deselect: function(){
      this.selected_cell = -1;
      this.selected_column = -1;
    },

    get_data: function(){
      let fields = this.fields
      return JSON.stringify(
        this.grid.map((row, index) => {
          let hash = {}
          row.map((cell, index) => {
            let key = fields[index] 
            hash[key] = cell || null
            return cell
          })
          return hash
        })
      )
    },

    add_row: function (row_after_index)  {
      this.grid.splice(row_after_index, 0, this.emptyRow())      
    },

    clean_table: function() {
      this.grid = this.emptyGrid(this.grid.length) 
    }
  }    
};
</script>

<style>
td {
  height: 40px;
  max-width: 5px; 
}

td > input {
  width: 100%;
  margin: -2px;
}

thead {
  padding-top: 20px 
}

.hide-headers .headers{
  display:none 
}

.highlight-odd tbody>tr:nth-of-type(odd){
  background-color: #f2f2f2
}

.highlight-even tbody>tr:nth-of-type(even){
 background-color: #f2f2f2
}
</style>
