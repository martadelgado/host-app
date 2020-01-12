<template>
  <section>
    <div v-for="hostname in uniqueHosts" :key="hostname">
      <div class="block__host-grid block__host" v-if="!checked">
        <h2 class="title__host">{{hostname}}</h2> 
        <ul class="list__host">
          <li class="item__host-grid" v-for="(hostData, index) in getTopAppsByHost(hostname, hostsData).slice(0, 5)" :key="index" @click="showVersionAlert(hostData.version)"  :id="hostData.version"><span class="score__host">{{ hostData.apdex }}</span><span class="name__host">{{ hostData.name }} - {{ hostData.contributors.join(', ') }}</span></li>
        </ul>
      </div>
      <div class="block__host-list block__host" v-if="checked">
        <h2 class="title__host">{{hostname}}</h2> 
        <ul class="list__host">
          <li class="item__host-grid" v-for="(hostData, index) in getTopAppsByHost(hostname, hostsData).slice(0, 5)" :key="index"><span class="score__host"  :id="hostData.version">{{ hostData.apdex }}</span><span class="name__host">{{ hostData.name }} - {{ hostData.contributors.join(', ') }}</span></li>
        </ul>
      </div>
    </div>
  </section>
</template>

<script>
import HostsData from '../assets/host-app-data.json'
  
export default {
  name: 'HostCard',
  data() {
    return {
      hostsData: HostsData,
      hostNames: [],
      uniqueHosts: [],
      hostArray: [],
    }
  },
  props: {
    checked: Boolean
  },
  created() {
    this.getAllHosts();
    this.getHostName(this.uniqueHosts);
  },
  methods: {
    addAppToHosts(host) {
      let hostObject = JSON.parse(host);
      return this.hostsData.push(hostObject);
    },
    removeAppFromHosts(host) {
      this.hostsData.filter(function(hostObj) {
        return hostObj.name !== host;
      })
    },
    getAllHosts() {
      var hostData;
      for (hostData = 0; hostData < this.hostsData.length; hostData++) {
        this.hostNames.push(this.hostsData[hostData].host);
      }
      return this.uniqueHosts = [...new Set(this.hostNames.flat())];
    },
    getHostName(arrayOfHosts) {
      return arrayOfHosts.map(name => { 
        this.getTopAppsByHost(name, this.hostsData);
      })
    },  
    getTopAppsByHost(host, hostData) {
      let array;

      function createNewArray() {
        function matchHost(elem) {
          return elem.host == host;
        }  
        return array = hostData.filter(matchHost);
      }

      function compare(a, b) {
        if (a.apdex > b.apdex)
          return -1;
        if (a.apdex < b.apdex)
          return 1;
        return 0;
      }
      createNewArray(); 
      return array.slice(0, 25).sort(compare);
    },
    showVersionAlert(version){
      alert(version)
    }
  }
}
</script>

<style scoped>
div {
  display: inline-block;
}

.block__host {
  background-color: #fff;
  display: inline-block;
  font-size: 16px;
  margin: 0 0 30px 30px;
  padding: 30px;
}

.block__host-grid {
  width: 315px;
}

.block__host-list {
  width: 720px;
}

.item__host-grid {
  color: #4A4A4A;
  line-height: 19px;
}

.item__host-grid:not(:last-of-type) {
  margin-bottom: 23px;
}

.list__host {
  list-style-type: none;
  margin-bottom: 0;
  padding-left: 0;
}

.name__host {
  font-size: 16px;
}

.score__host {
  font-size: 13px;
  margin-right: 23px;
  opacity: 0.62;
}

.title__host {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 23px;
  margin-top: 0;
}

section {
  display: flex;
  flex-wrap: wrap;
}
</style>
