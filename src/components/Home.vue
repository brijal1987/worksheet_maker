<template>
  <div class="container">
      <h2 class="headline mb-3">Please Create a Worksheet?</h2>
      <p class="error" v-if="errors.length">
        <b>Please correct the following error(s):</b>
        <ul>
          <li v-for="(error,index) in errors"  v-bind:key="index">{{ error }}</li>
        </ul>
      </p>
      <form @submit="submit">
        <div class="form-group">
          <label class="form-label">Word: *</label>
          <input class="form-control" placeholder="Enter Word"
              v-model="word"
              name="email"
              type="text">
        </div>
        <div class="form-group">
          <label class="form-label">Defination: *</label>
          <input class="form-control" placeholder="Enter definition"
              v-model="definition"
              name="definition"
              type="text">
        </div>
        <button type="submit">Add Word</button>
      </form>
      <hr/>
      <div>
          <h3>Your List:</h3>
          <table class="table">
              <tr>
                <th>Word</th>
                <th>Defination</th>
              </tr>
              <tr v-if="lists.length > 0" v-for="(list, i) in lists" :key="i">
                  <td>
                  {{list.word}}
                  </td>
                  <td>
                      {{list.definition}}
                  </td>
              </tr>
              <tr v-if="lists.length === 0" >
                <td colspan="2">No List Found</td>
              </tr>
          </table>
            <button class="print" v-if="lists.length > 0" type="submit"  @click.stop="printWorksheet">Print Worksheet</button>
      </div>
  </div>
</template>

<script>
import jsPDF from 'jspdf'
export default {
  name: 'Nav',
  data () {
    return {
      word: '',
      definition: '',
      lists: [],
      errors: []
    }
  },
  methods: {
    submit (e) {
      this.errors = []

      if (!this.word) {
        this.errors.push('Word required.')
      }
      if (!this.definition) {
        this.errors.push('Defination required.')
      }

      if (!this.errors.length) {
        this.lists.push({
          word: this.word,
          definition: this.definition
        })
        this.word = ''
        this.definition = ''
      }

      e.preventDefault()
    },
    getDateTime () {
      const today = new Date()
      const date = today.getFullYear() + '-' + (today.getMonth() + 1) + '-' + today.getDate()
      const time = today.getHours() + ':' + today.getMinutes() + ':' + today.getSeconds()
      return date + ' ' + time
    },
    printWorksheet (e) {
      e.preventDefault()
      // eslint-disable-next-line new-cap
      var doc = new jsPDF()
      let x = 10
      let y = 10
      this.lists.forEach(list => {
        doc.text(list.word, x, y)
        x = x + 100
        doc.text(list.definition, x, y)
        x = 10
        y = y + 10
      })
      doc.save('worksheet-' + this.getDateTime() + '.pdf')
    }
  }
}
</script>
<style>
body {
  font-family: "Open Sans"
}
.container {
  width:50%;
  background-color: #CCC;
  padding: 50px 100px;
  text-align:left;
}
form input {
  color: #ccc;
  border-radius: 2px solid
}
form button {
  background-color: #715DD6;
  border-radius: 5px solid #715DD6;
  color:#fff;
}
.print {
  background-color: #00B36F;
  border-radius: 5px solid #00B36F;
  color:#fff;
}
.error {
  color: #DC3545;
}
.headline {
  color: #715DD6;
}
</style>
