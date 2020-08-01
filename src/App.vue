<template>
  <div id="app">
    <div class="fixed">
      <Header/>

      <div class="container filtros">
        <button @click="getTodasOperadoras()" class="button todos">Todas</button>

        <select class="select select01" @change="getOperadorasPorModalidade()" v-model="modalidadeSelecionada" name="modalidade" id="modalidade">
          <option value="modalidade">Por Modalidade</option>
          <option
            v-for="modalidade in getModalidades"
            :key="modalidade"
            :value="modalidade"
          >{{modalidade}}</option>
        </select>

        <select class="select select01"
          @change="getOperadorasPorCidade()"
          v-model="cidadeSelecionada"
          name="cidade"
          id="cidade"
        >
          <option value="cidade">Por Cidade</option>
          <option v-for="cidade in getCidades" :key="cidade" :value="cidade">{{cidade}}</option>
        </select>

        <select class="select select01"
          @change="getOperadorasPorEstado()"
          v-model="estadoSelecionado"
          name="estado"
          id="estado"
        >
          <option value="estado">Por Estado</option>
          <option v-for="estado in getEstados" :key="estado" :value="estado">{{estado}}</option>
        </select>
      </div>
    </div>

    <main>
      <div class="container">
      
        <h2 v-if="mostrarOperadoraPorModalidade" class="tituloFiltro">Modalidade: {{modalidadeSelecionada}}</h2>
        <h2 v-if="mostrarOperadoraPorCidade" class="tituloFiltro">Cidade: {{cidadeSelecionada}}</h2>
        <h2 v-if="mostrarOperadoraPorEstado" class="tituloFiltro">Estado: {{estadoSelecionado}}</h2>
        <table>
          <thead>
            <th>Nº</th>
            <th>Razão Social</th>
            <th class="ocultarDispMoveis">CNPJ</th>
            <th class="ocultarDispMoveis">Modalidade</th>
            <th class="ocultarDispMoveis">Cidade</th>
            <th class="ocultarDispMoveis">Estado</th>
            <th>Mais informações</th>
          </thead>

          <tbody v-if="mostrarTodasOperadoras">
            <tr v-for="(operadora, index) in operadoras" :key="operadora.CNPJ">
              <td>{{index+1}}</td>
              <td>{{operadora["Razão Social"]}}</td>
              <td class="ocultarDispMoveis">{{operadora["CNPJ"]}}</td>
              <td class="ocultarDispMoveis">{{operadora["Modalidade"]}}</td>
              <td class="ocultarDispMoveis">{{operadora["Cidade"]}}</td>
              <td class="ocultarDispMoveis">{{operadora["UF"]}}</td>
              <td>
                <a @click="getMaisInformacoesOperadora(operadora.CNPJ)" class="ver-mais" href="#">
                 <i class="fa fa-search" aria-hidden="true"></i>
                </a>
              </td>
            </tr>
          </tbody>

          <tbody v-if="mostrarOperadoraPorModalidade">
            <tr v-for="(operadora, index) in operadorasPorModalidade" :key="operadora.CNPJ">
              <td>{{index+1}}</td>
              <td>{{operadora["Razão Social"]}}</td>
              <td class="ocultarDispMoveis">{{operadora["CNPJ"]}}</td>
              <td class="ocultarDispMoveis">{{operadora["Modalidade"]}}</td>
              <td class="ocultarDispMoveis">{{operadora["Cidade"]}}</td>
              <td class="ocultarDispMoveis">{{operadora["UF"]}}</td>
              <td>
                <a class="ver-mais" href="#">
                 <i class="fa fa-search" aria-hidden="true"></i>
                </a>
              </td>
            </tr>
          </tbody>

          <tbody v-if="mostrarOperadoraPorCidade">
            <tr v-for="(operadora, index) in operadorasPorCidade" :key="operadora.CNPJ">
              <td>{{index+1}}</td>
              <td>{{operadora["Razão Social"]}}</td>
              <td class="ocultarDispMoveis">{{operadora["CNPJ"]}}</td>
              <td class="ocultarDispMoveis">{{operadora["Modalidade"]}}</td>
              <td class="ocultarDispMoveis">{{operadora["Cidade"]}}</td>
              <td class="ocultarDispMoveis">{{operadora["UF"]}}</td>
              <td>
                <a class="ver-mais" href="#">
                 <i class="fa fa-search" aria-hidden="true"></i>
                </a>
              </td>
            </tr>
          </tbody>

           <tbody v-if="mostrarOperadoraPorEstado">
            <tr v-for="(operadora, index) in operadorasPorEstado" :key="operadora.CNPJ">
              <td>{{index+1}}</td>
              <td>{{operadora["Razão Social"]}}</td>
              <td class="ocultarDispMoveis">{{operadora["CNPJ"]}}</td>
              <td class="ocultarDispMoveis">{{operadora["Modalidade"]}}</td>
              <td class="ocultarDispMoveis">{{operadora["Cidade"]}}</td>
              <td class="ocultarDispMoveis">{{operadora["UF"]}}</td>
              <td>
                <a class="ver-mais" href="#">
                 <i class="fa fa-search" aria-hidden="true"></i>
                </a>
              </td>
            </tr>
          </tbody>
        </table>

        <div v-show="abrirModal" class="overflow-modal"></div>
        <div v-show="abrirModal" class="modal">
          <button @click="fecharModal()" class="botao-fechar-modal">X</button>
          <div>
            <p>Registro ANS: <span class="destaque">{{operadoraEspecifica["Registro ANS"]}}</span> </p>
            <p>Data Registro ANS: <span class="destaque">{{operadoraEspecifica["Data Registro ANS"]}}</span> </p>
            <p>Razão Social: <span class="destaque">{{operadoraEspecifica["Razão Social"]}}</span> </p>
            <p v-if="operadoraEspecifica['Nome Fantasia']">Nome Fantasia: <span class="destaque">{{operadoraEspecifica["Nome Fantasia"]}}</span> </p>
            <p>CNPJ: <span class="destaque">{{operadoraEspecifica["CNPJ"]}}</span> </p>
            <p>Modalidade: <span class="destaque">{{operadoraEspecifica["Modalidade"]}}</span> </p>
            <p>Representante: <span class="destaque">{{operadoraEspecifica["Representante"]}}</span> </p>
            <p>Cargo Representante: <span class="destaque">{{operadoraEspecifica["Cargo Representante"]}}</span> </p>
            <p>Telefone: <span class="destaque">({{operadoraEspecifica["DDD"]}}) {{operadoraEspecifica["Telefone"]}}</span> </p>
            <p v-if="operadoraEspecifica['Fax']">Fax: <span class="destaque">{{operadoraEspecifica["Fax"]}}</span> </p>
            <p>Endereço eletrônico: <span class="destaque">{{operadoraEspecifica["Endereço eletrônico"]}}</span> </p>
            <p>Endereço: <span class="destaque"> {{operadoraEspecifica["Logradouro"]}} Nº{{operadoraEspecifica["Número"]}} <span v-if="operadoraEspecifica['Complemento']"> - {{operadoraEspecifica["Complemento"]}}</span></span> </p>
            <p>Bairro: <span class="destaque">{{operadoraEspecifica["Bairro"]}}</span> </p>
            <p>CEP: <span class="destaque">{{operadoraEspecifica["CEP"]}}</span> </p>
            <p>Cidade: <span class="destaque">{{operadoraEspecifica["Cidade"]}}</span> Estado: <span class="destaque">{{operadoraEspecifica["UF"]}}</span> </p>
          </div>
        </div>

      </div>
    </main>
  </div>
</template>

<script>
import Header from './components/Header';
import '@fortawesome/fontawesome-free/css/all.css';
import '@fortawesome/fontawesome-free/js/all.js';
export default {
  components: {Header},
  data: () => {
    return {
      operadoras: {},
      modalidadeSelecionada: "modalidade",
      cidadeSelecionada: "cidade",
      estadoSelecionado: "estado",
      operadorasPorModalidade: {},
      operadorasPorCidade: {},
      operadorasPorEstado: {},
      operadoraEspecifica: {},
      mostrarTodasOperadoras: true,
      mostrarOperadoraPorModalidade: false,
      mostrarOperadoraPorCidade: false,
      mostrarOperadoraPorEstado: false,
      abrirModal: false
    };
  },
  methods: {
    getOperadoras() {
      fetch("/data/cadop.json")
        .then((resp) => resp.json())
        .then((resp) => (this.operadoras = resp));
    },
    filtrarCampos(chaveCampo) {
      let campo = Array.from(this.operadoras);
      campo = campo.map((operadora) => operadora[chaveCampo]);
      campo = Object.values(campo);
      let novaLista = [];
      campo.forEach((c) => {
        if (!novaLista.includes(c)) {
          novaLista.push(c);
        }
      });
      return novaLista.sort();
    },
    getOperadorasPorFiltro(filtro, select) {
      return this.operadoras.filter(
        (operadora) => operadora[filtro] === select
      );
    },
    getOperadorasPorModalidade() {
      this.mostrarTodasOperadoras = false;
      this.mostrarOperadoraPorCidade = false;
      this.mostrarOperadoraPorEstado = false;
      this.mostrarOperadoraPorModalidade = true;
      this.cidadeSelecionada = "cidade";
      this.estadoSelecionado = "estado";
      this.operadorasPorModalidade = this.getOperadorasPorFiltro(
        "Modalidade",
        this.modalidadeSelecionada
      );
    },
    getOperadorasPorCidade() {
      this.mostrarTodasOperadoras = false;
      this.mostrarOperadoraPorCidade = true;
      this.mostrarOperadoraPorEstado = false;
      this.mostrarOperadoraPorModalidade = false;
      this.modalidadeSelecionada = "modalidade";
      this.estadoSelecionado = "estado";
      this.operadorasPorCidade = this.getOperadorasPorFiltro(
        "Cidade",
        this.cidadeSelecionada
      );
    },
    getOperadorasPorEstado() {
      this.mostrarTodasOperadoras = false;
      this.mostrarOperadoraPorCidade = false;
      this.mostrarOperadoraPorEstado = true;
      this.mostrarOperadoraPorModalidade = false;
      this.modalidadeSelecionada = "modalidade";
      this.cidadeSelecionada = "cidade";
      this.operadorasPorEstado = this.getOperadorasPorFiltro(
        "UF",
        this.estadoSelecionado
      );
    },
    getTodasOperadoras(){
      this.mostrarTodasOperadoras = true;
      this.mostrarOperadoraPorCidade = false;
      this.mostrarOperadoraPorEstado = false;
      this.mostrarOperadoraPorModalidade = false;
      this.modalidadeSelecionada = "modalidade";
      this.cidadeSelecionada = "cidade";
      this.estadoSelecionado = "estado";
    },
    getMaisInformacoesOperadora(cnpj){
      this.abrirModal = true;
      this.operadoraEspecifica = this.operadoras.find(operadora => operadora.CNPJ === cnpj)
    },
    fecharModal(){
      this.abrirModal = false;
    }

  },
  computed: {
    getCidades() {
      return this.filtrarCampos("Cidade");
    },
    getEstados() {
      return this.filtrarCampos("UF");
    },
    getModalidades() {
      return this.filtrarCampos("Modalidade");
    },
  },
  mounted(){
    this.getOperadoras();
  }
};
</script>

<style>
:root{
  --cor01: #202020ee;
  --cor02: #f5f3f3;
  --cor03: #EF8354;
  --cor04: #1b1b1b;
  --cor05: #e4e1e1;
  --cor06: #747272;
  --cor07: #33363d;
  --font-size01: .8em;
  --font-size02: 1em;
  --font-size03: 1.5em;
  --font-size04: 1.3em;
  --height-buttons-selects: 40px;
  --border-radius: 5px;
}
* {
  margin: 0;
  padding: 0;
  font-family: "Roboto", sans-serif;
  box-sizing: border-box;
}
.container {
  width: 95%;
  margin-left: auto;
  margin-right: auto;
}
.fixed {
  position: fixed;
  width: 100%;
  background-color: var(--cor01);
  z-index: 2;
}
.filtros {
  margin-bottom: 50px;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
}
.button{
    display: block;
    border-radius: var(--border-radius);
    padding: 5px 10px;
    outline: none;
    border: none;
    cursor: pointer;
    font-size: var(--font-size02);
    height: var(--height-buttons-selects);
}
.button.todos{
    background-color:var(--cor03);
    color: var(--cor02);
    width: 10%;
}
.select{
  border-radius: var(--border-radius);
  padding: 5px 10px;
  outline: none;
  border: none;
  font-size: var(--font-size01);
  height: var(--height-buttons-selects);
}
.select01{
  background-color: var(--cor02);
}
main {
  position: relative;
  top: 250px;
}

.tituloFiltro{
  display: none;
}

table {
  border-collapse: collapse;
  text-align: center;
  border-radius: 5px;
  width: 90%;
  margin-left: auto;
  margin-right: auto;
}
thead {
  background-color: var(--cor07);
  color: var(--cor02);
  font-size: var(--font-size02);
}
tr,
thead, td {
  border: 1px solid var(--cor07);
}

tr {
  color: var(--cor04);
  background-color: var(--cor05);
  font-size: var(--font-size01);
  transition: background-color 1s ease-in-out, color 1.2s ease-in-out;
  height: 45px;
}
tr:hover{
  color:var(--cor02);
  background-color: var(--cor06);
}
tr:hover .ver-mais{
  color:var(--cor02);
}
td,
th {
  padding: 5px;
}
.ver-mais{
  color: var(--cor04);
  transition: color 1.5s ease-in-out
}
.ver-mais i{
  font-size: var(--font-size02);
}

.overflow-modal{
  position: fixed;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100vw;
  background-color: #5c5c5cdc;
  z-index: 3;
}

.modal{
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 50%;
  height: 400px;
  padding: 2%;
  background-color: var(--cor04);
  color: var(--cor02);
  z-index: 4;
  overflow-y: scroll;
  border: 3px solid var(--cor03);
  border-radius: var(--border-radius);
}

.modal div{
  margin-left: 50px;
}

.modal p{
  margin-bottom: 10px;
  width: 90%;
}

.botao-fechar-modal{
  display: block;
  border: none;
  outline: none;
  background-color: var(--cor02);
  width: 40px;
  height: 40px;
  padding: 1%;
  font-size: var(--font-size03);
  color: var(--cor03);
  position: fixed;
  left: 0px;
  top: 0;
  cursor: pointer; 
}

.destaque{
  color: var(--cor03);
}

@media screen and (max-width: 768px){
    main{
      top:400px;
    }

    .filtros{
      flex-direction: column;
    }

    .button.todos, .select01{
      margin-bottom: 20px;
      width: 30%;
    }

    .ocultarDispMoveis{
      display: none;
    }

    .tituloFiltro{
      display: block;
      color: var(--cor01);
      font-size: var(--font-size04);
      text-align: center;
      margin: 40px 0;
    }
}

@media screen and (max-width: 414px){
  .button.todos, .select01{
      width: 90%;
      min-width: 400px;
    }
}
</style>