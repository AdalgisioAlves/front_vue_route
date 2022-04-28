<template>
  <div class="CadastraProfissional">
    <b-container>
      <b-card>
        <div>
          <b-form>
            <b-form-group id="nome" label="Digite o Nome:" label-for="nome">
              <b-form-input
                id="nome"
                v-model="nome"
                type="text"
                placeholder="Digite o Nome"
                required
              ></b-form-input>
            </b-form-group>
              <b-form-group id="telefone" label="Digite o Telefone:" label-for="telefon">
              <b-form-input
                id="telefone"
                v-model="telefone"
                type="text"
                placeholder="Digite o Telefone"
                required
              ></b-form-input>
            </b-form-group>
            <b-form-group
              id="crm"
              label="Digite CRM:"
              label-for="crm"
            >
              <b-form-input
                id="crm"
                v-model="crm"
                placeholder="Digite CRM"
                required
              ></b-form-input>
            </b-form-group>
            <b-form-group>
              <b-form-select
                id="especialidades"
                :options="objEspecialidades"
                value-field="item"
                text-field="name"
                required
                multiple
                v-model="especialidades"
              >
              </b-form-select>
            </b-form-group>
            <b-form-group id="input-group-4"> </b-form-group>
            <b-button variant="primary" v-on:click="salvarProfissional">Salvar</b-button>
          </b-form>
          <b-alert class="{{tipoAleta}}" show v-if="mensagem">
          {{mensagem}}
          </b-alert>
        </div>
      </b-card>
    </b-container>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "CadastraProfissional",
  components: {},
  data() {
    return {
      objEspecialidades: [],
      mensagem: '',
      tipoAleta: null
    };
  },
  methods: {
    buscarProfissional: function () {
      axios
        .get("http://localhost/api/config/especialidade")
        .then((response) => {     
          let objEsp  = []     
          response.data.data.forEach((el) => {
            objEsp.push({item: el.id, name: el.especialidade})
          });
          this.objEspecialidades = objEsp
        })
        .catch((error) => {
          console.log(error);
          this.errored = true;
        })
        .finally(() => (this.loading = false));
    },
    salvarProfissional: function(){
      let formData = {
        nome:this.nome,
        telefone: this.telefone,
        crm: this.crm,
        especialidades: this.especialidades
      }
     
      axios
        .post("http://localhost/api/profissional/post", formData)
        .then((response) => {
          let ret = response.data
          console.log(ret)
          if(ret.erro){
            this.mensagem = ret.msg + ' - ' +ret.erro
            this.tipoAleta = 'danger'
          }
          else{
            this.mensagem = ret.msg
            this.tipoAleta = 'success'
          }
          
        })
        .catch((error) => {
          console.log(error);
          this.errored = true;
        })
        .finally(() => (this.loading = false));
      console.log(this.especialidades)

    },
  },
  created() {
    this.buscarProfissional();
  },
};
</script>
