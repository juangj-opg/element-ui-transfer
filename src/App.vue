
<template>
  <div>
    <h2 style="text-align: center; margin: 20px 0 20px">Notificaciones</h2>
    <div style="text-align: center">
      <el-transfer ref="transfer"
        style="text-align: left; display: inline-block"
        v-model="value"
        :data="data">
        <span slot-scope="{ option }" class="option">
          <b>{{ option.label }} </b>
          <span v-if="value.includes(option.key)">
            <font-awesome-icon icon="fa-solid fa-lock" v-on:click="removeChannel(option)" v-if="requiredChannels.includes(option)" /> 
            <font-awesome-icon icon="fa-solid fa-lock-open" v-on:click="addChannel(option)" v-else/> 
          </span>
        </span>
      </el-transfer>

    <h2>Lista en la derecha</h2>
      <ul>
        <li v-for="item in requiredChannels">
          {{item.label}}
        </li>
      </ul>

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
            key: index,
          })
        });
        return data;
      };
      return {
        data: generateData(),
        value: [],
        requiredChannels: [],
      };
    },

    methods: {
      addChannel(item) {
        this.requiredChannels.push(item);
        console.log(item);

      },
      removeChannel(item) {
        this.requiredChannels.splice(this.requiredChannels.findIndex(c => c.key === item.key), 1);
        console.log(item);

      }
    }
  };
  
</script>

