<template>
  <div>
    <div id="add_btn" class="container">
      <div class="row">
        <div class="col-9">
          <b-form-input
            id="search"
            size="sm"
            class="mr-sm-2 search"
            placeholder="Digite aqui para buscar..."
          ></b-form-input>
          <b-button
            size="sm"
            class="my-2 my-sm-0 search"
            type="submit"
            variant="outline-secondary"
          >Buscar</b-button>
        </div>
        <div class="col-3">
          <b-button
            size="sm"
            class="my-2 my-sm-0"
            variant="outline-primary"
            v-b-modal.modal-prevent-closing
          >Cadastrar</b-button>
        </div>
      </div>
    </div>
    <pessoas v-bind:pessoas="pessoas" v-on:del-pessoa="deletepessoa" />

    <b-modal
      id="modal-prevent-closing"
      ref="modal"
      title="Cadastro de Produtos"
      @show="resetModal"
      @hidden="resetModal"
      @ok="handleOk"
      centered
    >
      <form ref="form" @submit.stop.prevent="addPessoa">
        <b-form-group
          :state="state"
          label="Nome"
          label-for="name-input"
          invalid-feedback="Nome é obrigatório"
        >
          <b-form-input id="name-input" v-model="name" :state="state" required></b-form-input>
        </b-form-group>
        <b-form-group
          :state="state"
          label="Email"
          label-for="email-input"
          invalid-feedback="Email é obrigatório"
        >
          <b-form-input id="email-input" v-model="email" :state="state" required></b-form-input>
        </b-form-group>
        <b-form-group
          :state="state"
          label="Endereço"
          label-for="endereco-input"
          invalid-feedback="Endereço é obrigatório"
        >
          <b-form-input id="endereco-input" v-model="endereco" :state="state" required></b-form-input>
        </b-form-group>
        <b-form-group
          :state="state"
          label="Sexo"
          label-for="sexo-input"
          invalid-feedback="Sexo é obrigatória"
        >
          <b-form-input id="sexo-input" v-model="sexo" :state="state" required></b-form-input>
        </b-form-group>
        <b-form-group
          :state="state"
          label="Ativo"
          label-for="ativo-input"
          invalid-feedback="Atvo é obrigatória"
        >
          <b-form-input id="ativo-input" v-model="ativo" :state="state" required></b-form-input>
        </b-form-group>
      </form>
    </b-modal>
  </div>
</template>

<script>
import pessoas from "./pessoas";
import OwnerService from '@/api-services/owner.service';

export default {
  name: "Home",
  //teste
   created() {
    OwnerService.getAll().then((response) => {
      console.log(response.data);
    }).catch((error) => {
      console.log(error.response.data);
    });
  },
  //////
  components: {
    pessoas
  },
  data() {
    return {
      name: "",
      email: "",
      endereco: "",
      sexo: "",
      ativo: "",
      state: null,
      pessoas: []
    };
  },
  
  methods: {
    deletepessoa(id) {

    },
    checkFormValidity() {
      const valid = this.$refs.form.checkValidity();
      this.state = valid ? "valid" : "invalid";
      return valid;
    },
    resetModal() {
      this.name = "";
      this.email = "";
      this.endereco = "";
      this.sexo = "";
      this.ativo = "";
      this.state = null;
    },
    handleOk(bvModalEvt) {
      bvModalEvt.preventDefault();
      this.addPessoa();
    },
    addPessoa() {
      if (!this.checkFormValidity()) {
        return;
      }
      console.log('deu certo');
      let newpessoa = {
        name: this.name,
        email: this.email,
        endereco: this.endereco,
        sexo: this.sexo,
        ativo: this.ativo
      };
      //POST PARA MANDAR PRO BANCO DE DADOS
       /* axios.request ({
        url: 'http://localhost:3000/',
        method: 'post',
        data: newpessoa,
        headers: {'X-Requested-With': 'XMLHttpRequest'},
        // bd
        }) .then(function (response) {
            console.log(response);
        })
        .catch(function (error) {
            console.log(error);
        });*/
       this.pessoas.push(newpessoa);
       this.$nextTick(() => {
        this.$refs.modal.hide();
      });
    }
  }
};
</script>

<style>
footer {
  display: block;
}
#add_btn {
  padding: 30px;
  border-width: 0.2rem 0 0;
  text-align: right;
}
#search {
  width: 50%;
  display: inline;
}
.modal-backdrop {
  background-color: rgba(0, 0, 0, 0.473);
}
</style>