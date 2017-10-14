<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    {{todate0}}
    <md-layout md-align="center" md-gutter>
      <md-layout md-flex="30">
        <md-input-container>
          <label>Name Notes</label>
          <md-input v-model="newNote.name" ></md-input>
        </md-input-container>
        <md-input-container>
          <label>Detail</label>
          <md-textarea v-model="newNote.detail" maxlength="500"></md-textarea>
          <input type="hidden" v-model="newNote.date" v-bind:value="todate">
        </md-input-container>
        <md-button v-on:click="addnote()" class="md-raised md-primary">Add Note</md-button>
      </md-layout>
    </md-layout>
    <md-layout md-align="center" md-gutter>
      <h1>
        My Notes
      </h1>

      <md-layout md-flex="100"> 
        <div v-for="(a, index) in notes">
          <md-card>
          <md-card-header>
            <div class="md-title">{{a.name}}</div>
            <div class="md-subhead">{{a.date}}</div>
          </md-card-header>
          <md-card-actions>
            <md-button v-on:click="shownote(index)">Show</md-button>
            <md-button v-on:click="delnote(index)">Delete</md-button>
          </md-card-actions>
        </md-card>
        </div>
      </md-layout>
    </md-layout>
    <H2>Content Notes</H2>
    <p class="hide" id="notedetail" align="center"></p>
        
  </div>
</template>

<script>
import Firebase from 'firebase'
// var firebase = require('firebase')
  // Initialize Firebase
var config = {
  apiKey: 'AIzaSyDZiGEHS_dMBgz43QBCCdoKN5KX-Rulc6U',
  authDomain: 'vue-project-4a8f5.firebaseapp.com',
  databaseURL: 'https://vue-project-4a8f5.firebaseio.com',
  projectId: 'vue-project-4a8f5',
  storageBucket: 'vue-project-4a8f5.appspot.com',
  messagingSenderId: '232717547987'
}
let app = Firebase.initializeApp(config)
let db = app.database()
let noteref = db.ref('notes')
var timeInMs = new Date()
var todate = timeInMs.getDate() + '/' + (timeInMs.getMonth() + 1) + '/' + timeInMs.getFullYear()
export default {
  name: 'HelloWorld',
  firebase: {
    notes: noteref
  },
  data () {
    return {
      msg: 'VUE - Note JS',
      todate0: todate,
      newNote: {
        name: '',
        detail: '',
        date: todate
      }
    }
  },
  methods: {
    addnote: function () {
      noteref.push(this.newNote)
      this.newNote.name = ''
      this.newNote.detail = ''
      this.newNote.date = this.todate
    },
    delnote: function (index) {
      this.notes.splice(index, 1)
    },
    shownote: function (index) {
      document.getElementById('notedetail').innerHTML = this.notes[index].detail
      var x = document.getElementById('notedetail')
      if (x.style.display === 'none') {
        x.style.display = 'block'
      } else {
        x.style.display = 'block'
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.hide {
  display: none;
  border: 2px solid black;
  width: 50%;
  min-height: 200px;
  text-align: left;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 50px;
}
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
