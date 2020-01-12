<template>
  <div id="app">
    <div class="container">
      <div class="block__title">
        <h1 class="title">Apps by Host </h1>
        <span class="sub-title"> for user averylongemailaddress@companyname.com</span>
        <span class="checkbox__hosts-view"><input type="checkbox" @click="changeCheckbox" v-model="checked"> {{ checkboxText }}</span>
      </div>
      <HostCard :checked="checked" @method="changeCheckbox"/>
    </div>
  </div>
</template>

<script>
import HostCard from './components/HostCard.vue'

export default {
  name: 'app',
  components: {
    HostCard
  },
  data() {
    return {
      checkboxText: 'Show as list',
      checked: false
    }
  },
  methods: {
    changeCheckbox() {
      this.checkboxText = 'Show as an awesome grid';
      if (this.checked) {
        this.checkboxText = 'Show as list';
      }
    }
  },
}
</script>

<style>
body {
  background-color: #eee;
  color: #000;
  font-family: 'HelveticaNeue', Helvetica, Arial, sans-serif;
}

.block__title {
  align-items: baseline;
  display: flex;
  justify-content: space-between;
  margin: 0 30px 30px;
}

.container {
  width: 840px;
}

.checkbox__hosts-view {
  white-space: nowrap;
}

.sub-title {
  font-size: 18px;
  line-height: 22px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.title {
  display: inline;
  font-size: 35px;
  font-weight: bold;
  line-height: 43px;
  margin-bottom: 0;
  margin-right: 6px;
  white-space: nowrap;
}
</style>
