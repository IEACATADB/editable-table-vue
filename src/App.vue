<template>
  <div id="app">
    <div v-if="initialized">
      <ETable  :rows='rows' :fields='fields' :meta='meta'/>
      <button @click="initialized=false">destroy</button>
    </div>
    <div v-else>
      <div>
        <input type="number" name="rows" min='1' v-model="rows" />
        <label for="rows">Количество строк</label>
      </div>
      <div>
        <input type="text" name="headers" v-model="headers" />
        <label for="headers">Заголовки через пробел</label>
      </div>
      <div>
        <input type="text" name="meta" v-model="meta"/>
        <label for="meta">Дополнительные css классы</label>
        <p>Доступные классы:</p>
        <ul>
          <li>hide-headers  Скрывает заголовки  </li>
          <li>highlight-even  Выделяет четные строки  </li>
          <li>highlight-odd  Выделяет нечетные строки </li>
        </ul>
      </div>
      <button @click="initialized=true">initialize</button>
    </div>
  </div>
</template>

<script>
import ETable from './components/Table'

export default {
  name: 'app',
  components: {
    ETable
  },
  data(){ 
   return {
     initialized: false,
     rows: 5,
     headers: '1 2 3 4 5',
     meta: ''
   }
  },
  computed:{
    fields: function(){
      return this.headers.split(' ')
    }
  }
}
</script>

<style>
/* sample text */
</style>
