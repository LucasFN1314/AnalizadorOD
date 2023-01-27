<template>
  <div>
    <input type="file" multiple v-on:change="read()" id="input_01">
    <button v-on:click="process()">Procesar</button>
    <br>
    <textarea id="resultRead" cols="100" rows="38"></textarea>
  </div>
</template>

<script>
/* eslint-disable */

class OD {
  constructor() {
    this.dnis = [];
    this.nombre = '';
  }

  addDni(dni) {
    this.dnis.push(dni);
  }
}

class ORDENES {
  
}


export default {
  data() {
    return {
      ordenes: [],
    }
  },
  mounted() {

  },
  methods: {
    async read() {
      let input_files = document.getElementById('input_01').files;

      for (let i = 0; i < input_files.length; i++) {
        this.readText(input_files[i]);
      }

    },
    async readText(file) {
      let fr = new FileReader();

      fr.onload = () => {
        let out = document.getElementById('resultRead');
        out.textContent = '';
        out.textContent += fr.result + '\n';
        for (let i = 0; i < 100; i++) { out.textContent += '*'; }
        out.textContent += '\n';


        this.process();

      };

      fr.readAsText(file);
    },
    process() {
      let text = document.getElementById('resultRead').textContent;
      let lines = text.split('\n');

      //var regex = ('/^[1-9]{2}.[0-9]{3}.[0-9]{3}$/i');
      var regex = ('/^[/]$/i');
      let number_order = '';


      lines.map((line) => {
        let splittedLine = line.split(' ');

        // Busqueda orden dia
        splittedLine.map((word) => {
          if (word == 'ARGERICH”') {
            let ind = splittedLine.indexOf('Nro');
            if (ind > -1) {
              number_order = splittedLine[ind] + ' ';
              if (splittedLine[ind + 1] != undefined) number_order += splittedLine[ind + 1];
              if (splittedLine[ind + 2] != undefined) number_order += splittedLine[ind + 2];
              if (splittedLine[ind + 3] != undefined) number_order += splittedLine[ind + 3];

              console.log(number_order);
              let ord = new OD();
              ord.nombre = number_order;
              this.ordenes.push(ord);

              this.ordenes.map((orden) => {
                console.log(orden);
              });

            }
          }
        });

        // Verify

      });
    }
  }
}
</script>

<style>

</style>