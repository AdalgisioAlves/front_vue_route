<template>
  <div class="hello">
    <b-container>
      <b-input-group prepend="Nome do Profissional" class="mt-3">
        <b-form-input v-model="nome"> </b-form-input>
        <b-input-group-append>
          <b-button variant="outline-success" v-on:click="buscarProfissional"
            >Pesquisar</b-button
          >
        </b-input-group-append>
      </b-input-group>

      <b-alert variant="success" show v-if="ativo">
        <h3>Profissional</h3>{{ profissional.nome }}<br>
        <h3>Telefone</h3>{{ profissional.telefone }}<br>
        <h3>CRM</h3>{{ profissional.crm }}
          <h3>Especialidades:</h3>
        <ol>
          <li v-for="especialidade in profissional.especialidades"  :key="especialidade.id">
            {{ especialidade.especialidade }}
          </li>
        </ol>
      </b-alert>
    </b-container>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      profissional: null,
      dados: null,
      ativo: false,
    };
  },
  methods: {
    buscarProfissional: function () {
      axios
        .post("http://localhost/api/profissional/buscar", {
          nome: this.nome,
        })
        .then((response) => {
          let ret = response.data
          if(ret.data.length > 0){
            console.log(ret)
            this.profissional = ret.data[0];
            this.ativo = true;
          }
          else{
            alert('Profissional Não encontrado')
            this.ativo = false;
          }
          
        })
        .catch((error) => {
          console.log(error);
          this.errored = true;
        })
        .finally(() => (this.loading = false));
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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
