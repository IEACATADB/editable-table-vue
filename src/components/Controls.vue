<template>
<tr>
  <th :colspan="length">
    <div class="btn-group btn-group-sm">
      <button class="btn btn-secondary" @click.stop.prevent='add_row'>добавить строку</button>
      <button class="btn btn-secondary" @click.stop.prevent='clear_table'>очистить таблицу</button>
      <button class="btn btn-secondary" @click.stop.prevent='get_data'>экспорт в JSON</button>
    </div>  
  </th>
</tr>
</template>
<script>
import {EventBus} from './EventBus.js'
export default {
  name: 'Controls',
  props:{
    length:Number
  },
  methods:{
    get_data: function(){
      EventBus.$emit('get_data')
    },
    clear_table: function(){
      EventBus.$emit('clear_table')
    },
    add_row: function(){
      EventBus.$emit('add_row', prompt('Строка после которой следует добавить пустую'))  
    }
  }
}
</script>