<template>
  <div id="app">
    {{number}}
    <button @click="cal(1)">+</button>
    <button @click="cal(-1)">-</button>
    Cust Po : <input v-model="cs"> Input
    <input v-model="inp" @input="test">
    <button @click="convertJsonToXlsx" class="btn btn-danger">Export Xlsx</button>
    <br>
    <div class="di">
      No.
      <div v-for="i in index">
        {{i.data}}
      </div>
    </div>
    <div class="di">
      Cust PO#
      <div v-for="(i, index) in cust">
        {{i.data}}
        <button @click="del(index, 'cust')">del line</button>
      </div>
    </div>
    <div class="di">
      Model
      <div v-for="(i, index) in model">
        {{i.data}}
        <button @click="del(index, 'model')">del</button>
      </div>
    </div>
    <div class="di">
      S/N
      <div v-for="(i, index) in sn">
        {{i.data}}
        <button @click="del(index, 'sn')">del</button>
      </div>
    </div>

  </div>
</template>

<script>
import Hello from './components/Hello'
window.onbeforeunload = function() {
  return "Noo!"
}
export default {
  name: 'app',
  data  () {
    return {
      inp: '',
      number: 1,
      data: {
        relations: [],
        cust: [],
        model: [],
        sn: []
      },
      cust: [],
      model: [],
      sn: [],
      indexC: 0,
      indexM: 0,
      indexS: 0,
      index: [],
      indexI: 0,
      cs: ''
    }
  },
  methods: {
    cal (n) {
      this.number += n
    },
    test () {
      if (this.inp.indexOf('KSS122') !== -1 && this.inp.length > 12) {
        // this.cust.push({data: this.inp})
        this.cs = this.inp
        console.log('c');
        this.inp = ''
      }
      if (this.inp.indexOf('1PXFP') !== -1 && this.inp.length === 19) {
        this.model.push({data: this.inp})
        console.log('m');
        this.inp = ''
      }
      if (this.inp.indexOf('SFNS') !== -1 && this.inp.length === 12) {
        this.sn.push({data: this.inp})
        console.log('s');
        this.inp = ''
      }
      if (this.model[this.indexI] && this.sn[this.indexI]) {
        this.cust.push({data: this.cs})
        this.index.push({data: this.number})
        this.indexI++
      }
    },
    del (index, name) {
      console.log(name)
      console.log(index)
      if (name === 'cust') {
        this.cust.splice(index, 1)
        this.model.splice(index, 1)
        this.sn.splice(index, 1)
        this.index.splice(index, 1)
        this.indexI--
      }
      if (name === 'model') {
        this.model.splice(index, 1)
      }
      if (name === 'sn') {
        this.sn.splice(index, 1)
      }
    },
    convertJsonToXlsx () {
      var data = this.data.relations
      console.log(data)
      var csvContent = 'data:text/csv;charset=utf-8,'
      var _this = this
      var tempSub = []
      var tempTech = []
      csvContent += 'No.,Cust PO#,Model,S/N\n'
      // data.forEach(function (infoArray, index) {
      //   var subject = _this.data.cust.find(subject => subject.id === infoArray.subject)
      //   var teacher = _this.data.model.find(techer => techer.id === infoArray.techer)
        // tempSub.push(subject)
        // tempTech.push(teacher)
        // csvContent += subject.code + ',' + subject.credit + ',' + subject.name + ',' + teacher.name + '\n'
        // console.log(csvContent)
      // })
      // tempSub = tempSub.sort((a, b) => (a.code - b.code))
      // tempTech = tempTech.sort((a, b) => (a.code - b.code))
      // tempSub.forEach(function (infoArray, index) {
      //   csvContent += tempSub + ',' + tempTech + '\n'
      //   console.log(csvContent)
      // })
      for (var i = 0; i < this.cust.length; i++) {
        console.log(this.cust[i].data)
        if (this.index[i - 1]) {
          if (this.index[i].data > this.index[i - 1].data) {
            csvContent += ', , , \n'
          }
        }
        csvContent += this.index[i].data + ',' +  this.cust[i].data  + ',' + this.model[i].data + ',' +  this.sn[i].data + '\n'
        console.log(csvContent)
      }
      var encodedUri = encodeURI(csvContent)
      var link = document.createElement('a')
      link.setAttribute('href', encodedUri)
      link.setAttribute('download', '2june2017.csv')
      document.body.appendChild(link)
      link.click()
    },
    // addRelation (techer, subject) {
    //  if (!this.data.relations.find(r => r.techer === techer && r.subject === subject)) {
    //    this.data.relations.push({
    //      techer,
    //      subject
    //    })
    //  }
  //  }
  },
  mounted () {
    // if (!localStorage.techers) {
    //   localStorage.techers = []
    // }
    // if (!localStorage.subjects) {
    //   localStorage.subjects = []
    // }
    // if (!localStorage.relations) {
    //   localStorage.relations = []
    // }
  },
  components: {
    Hello
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
div.di {
  float: left;
  padding: 10px;
}
</style>
