<template>
  <div class="container">
    <div>
      <h1 class="title">
        Project Board
      </h1>
      <button @click="addStatus()" v-if="statuses.length == 0" class="bg-green-700 text-white text-lg mt-8 px-4 py-2 rounded-lg transition hover:bg-green-900">
          Create New
      </button>
      <div class="flex flex-row flex-wrap">
        <StatusCol v-for="(status, idx) in statuses" :key="status" :addStatusFn="() => addStatus(idx + 1)"/>
      </div>
    </div>
  </div>
</template>

<script>
import { v4 as uuidv4 } from 'uuid';
export default {
  mounted() {
    this.statuses = JSON.parse(localStorage.getItem('statuses')) || [];
  },
  data() {
    return {
      statuses: []
    };
  },
  methods: {
    addStatus(index = 0) {
      console.log("clicked");
      let status = {
        title: 'status',
        color: '#C8C8C8',
        cards: []
      }
      let uuid = uuidv4();
      localStorage.setItem(uuid, JSON.stringify(status));
      this.statuses.splice(index, 0, uuid);
      localStorage.setItem('statuses', JSON.stringify(this.statuses));
    }
  }
}
</script>

<style>
/* Sample `apply` at-rules with Tailwind CSS
.container {
@apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/
.container {
  margin: 0 auto;
  height: 100vh;
  display: flex;
  justify-content: center;
  /* align-items: center; */
  text-align: center;
}

body {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  /* display: block;
  font-weight: 300; */
  font-size: 30px;
  color: #35495e;
  letter-spacing: 1px;
  overflow-y: auto;
}

.title {
  font-size: 72px;
}

/* .subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
} */

.links {
  padding-top: 15px;
}
</style>
