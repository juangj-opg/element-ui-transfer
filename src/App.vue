
<template>
  <div>
  <!--  
    <p style="text-align: center; margin: 0 0 20px">Customize data items using render-content</p>
    <div style="text-align: center">
      <el-transfer
        style="text-align: left; display: inline-block"
        v-model="value"
        filterable
        :left-default-checked="[]"
        :right-default-checked="[]"
        :render-content="renderFunc"
        :titles="['Source', 'Target']"
        :button-texts="['To left', 'To right']"
        :format="{
          noChecked: '${total}',
          hasChecked: '${checked}/${total}'
        }"
        @change="handleChange"
        :data="data">
        <el-button class="transfer-footer" slot="left-footer" size="small">Operation</el-button>
        <el-button class="transfer-footer" slot="right-footer" size="small">Operation</el-button>
      </el-transfer>
    </div>
  -->  
  
    <h2 style="text-align: center; margin: 20px 0 20px">Customize data items using scoped slot</h2>
    <div style="text-align: center">
      <el-transfer
        style="text-align: left; display: inline-block"
        v-model="value"
        filterable
        :left-default-checked="[]"
        :right-default-checked="[]"
        :titles="['No suscrito', 'Suscrito']"
        :button-texts="['Desuscribir', 'Suscribir']"
        :format="{
          noChecked: '${total}',
          hasChecked: '${checked}/${total}'
        }"
        @change="handleChange"
        :data="data">
        <span slot-scope="{ option }" class="option">
          <b>{{ option.label }} </b>
          <input type="checkbox"  :id="option.key"> 
          <label :for="option.key"> 
            <i class="fa-solid fa-lock"></i>
            <font-awesome-icon icon="fa-solid fa-lock" v-on:click="change" /> 
            <font-awesome-icon icon="fa-solid fa-lock-open" v-on:click="change" /> 

          </label>
        </span>
        <el-button class="transfer-footer" slot="left-footer" size="small">Operation</el-button>
        <el-button class="transfer-footer" slot="right-footer" size="small">Operation</el-button>
      </el-transfer>

    <h2>Lista en la derecha</h2>
      <ul id="rgt"></ul>

    <h2>Lista en la derecha marcados</h2>
      <ul id="chk"></ul>
    
    </div>
  </div>
</template>

<style>
@import url("//unpkg.com/element-ui@2.15.7/lib/theme-chalk/index.css");

  .transfer-footer {
    margin-left: 20px;
    padding: 6px 5px;
  }
</style>

<script>
  export default {
    data() {
      const generateData = _ => {
        const data = [];
        const notifys = ["Email", "Firebase", "SMS", "WhatsApp"];
        notifys.forEach((notify, index) => {
          data.push({
            label: notify,
            key: index
          })
        });
        return data;
      };
      return {
        data: generateData(),
        value: [],
        renderFunc(h, option) {
          return <span>{ option.key } - { option.label }</span> ;
        }
      };
    },

    methods: {
      handleChange(value, direction, movedKeys) {
        console.log(value, direction, movedKeys);
        
        var right = []
        /* 
         * Lee los valores que se han movido a la derecha
         * - 'value' devuelve la posición de cada uno de los items, 
         *    así que, a cada item se le asignó una id que corresponde
         *    al mismo valor que 'value' y de esa forma encontrarlo.
         */
        value.forEach(val => {
          // Pendiente de transformar a Vue si es posible
          const notify = document.getElementById(val)
                             .parentElement
                             .getElementsByTagName("b")[0]
                             .innerHTML;
          right[val] = notify; // * Estaba pensado en ordenar la lista con .sort, pero, no hay necesidad ninguna.
        })

        // * Limpiar la lista
        document.getElementById("rgt").innerHTML = "";

        // * Crea la lista con los elementos del array 
        right.forEach(notify =>{
          const node = document.createElement("li");
          const textnode = document.createTextNode(notify);
          node.appendChild(textnode);

          document.getElementById("rgt").appendChild(node);
        });
        
      },
      change: function (event){
        console.log(event.currentTarget.parentElement);
      }
    }
  };
  
</script>

