<template>
  <div class="container">
    <div>
      <h1 class="title">
        Project Board
      </h1>
      <div class="flex flex-row flex-wrap">
        <StatusCol v-for="status in statuses" :key="status"/>
      </div>
    </div>
  </div>
</template>

<script>
import { v4 as uuidv4 } from 'uuid';
export default {
  methods: {
    id() {
      return uuidv4();
    }
  },
  mounted() {
    this.statuses = JSON.parse(localStorage.getItem('statuses'));
  },
  data() {
    return {
      statuses: []
    };
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
