<template>
  <div id="app">
    <div class="fixed">
      <Header/>

      <div class="container filtros">
        <button @click="getTodasOperadoras()" class="button todos">Todas</button>

        <select class="select select01"
          @change="getOperadorasPorModalidade()"
          v-model="modalidadeSelecionada"
          name="modalidade"
          id="modalidade"
        >
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

        <table>
          <thead>
            <th>Nº</th>
            <th>Razão Social</th>
            <th>CNPJ</th>
            <th>Modalidade</th>
            <th>Cidade</th>
            <th>Estado</th>
            <th>Mais informações</th>
          </thead>

          <tbody v-if="mostrarTodasOperadoras">
            <tr v-for="(operadora, index) in operadoras" :key="operadora.CNPJ">
              <td>{{index+1}}</td>
              <td>{{operadora["Razão Social"]}}</td>
              <td>{{operadora["CNPJ"]}}</td>
              <td>{{operadora["Modalidade"]}}</td>
              <td>{{operadora["Cidade"]}}</td>
              <td>{{operadora["UF"]}}</td>
              <td>
                <a class="ver-mais" href="#">
                  <img src="/images/ver-mais.svg" alt="ver mais" />
                </a>
              </td>
            </tr>
          </tbody>

          <tbody v-if="mostrarOperadoraPorModalidade">
            <tr v-for="(operadora, index) in operadorasPorModalidade" :key="operadora.CNPJ">
              <td>{{index+1}}</td>
              <td>{{operadora["Razão Social"]}}</td>
              <td>{{operadora["CNPJ"]}}</td>
              <td>{{operadora["Modalidade"]}}</td>
              <td>{{operadora["Cidade"]}}</td>
              <td>{{operadora["UF"]}}</td>
              <td>
                <a class="ver-mais" href="#">
                  <img src="/images/ver-mais.svg" alt="ver mais" />
                </a>
              </td>
            </tr>
          </tbody>

          <tbody v-if="mostrarOperadoraPorCidade">
            <tr v-for="(operadora, index) in operadorasPorCidade" :key="operadora.CNPJ">
              <td>{{index+1}}</td>
              <td>{{operadora["Razão Social"]}}</td>
              <td>{{operadora["CNPJ"]}}</td>
              <td>{{operadora["Modalidade"]}}</td>
              <td>{{operadora["Cidade"]}}</td>
              <td>{{operadora["UF"]}}</td>
              <td>
                <a class="ver-mais" href="#">
                  <img src="/images/ver-mais.svg" alt="ver mais" />
                </a>
              </td>
            </tr>
          </tbody>

           <tbody v-if="mostrarOperadoraPorEstado">
            <tr v-for="(operadora, index) in operadorasPorEstado" :key="operadora.CNPJ">
              <td>{{index+1}}</td>
              <td>{{operadora["Razão Social"]}}</td>
              <td>{{operadora["CNPJ"]}}</td>
              <td>{{operadora["Modalidade"]}}</td>
              <td>{{operadora["Cidade"]}}</td>
              <td>{{operadora["UF"]}}</td>
              <td>
                <a class="ver-mais" href="#">
                  <img src="/images/ver-mais.svg" alt="ver mais" />
                </a>
              </td>
            </tr>
          </tbody>

        </table>
      </div>
    </main>
  </div>
</template>

<script>
import Header from './components/Header';

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

      mostrarTodasOperadoras: true,
      mostrarOperadoraPorModalidade: false,
      mostrarOperadoraPorCidade: false,
      mostrarOperadoraPorEstado: false,
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
      // Object.assign(this.$data, this.$options.data())
      this.mostrarTodasOperadoras = true;
      this.mostrarOperadoraPorCidade = false;
      this.mostrarOperadoraPorEstado = false;
      this.mostrarOperadoraPorModalidade = false;

      this.modalidadeSelecionada = "modalidade";
      this.cidadeSelecionada = "cidade";
      this.estadoSelecionado = "estado";
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

  mounted() {
    this.getOperadoras();
  },
};
</script>

<style>
:root{
  --cor01: #202020ee;
  --cor02: #e4e3e3;
  --cor03: #EF8354;
  --cor04: #1b1b1b;
  --cor05: #e4e1e1;
  --cor06: #747272;
  --cor07: #33363d;

  --font-size01: .8em;
  --font-size02: 1.1em;
  --font-size03: 1.5em;

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

.todos{
    background-color:var(--cor03);
    color: var(--cor02);
    width: 200px;
}

.select{
  border-radius: var(--border-radius);
  padding: 5px 10px;
  outline: none;
  border: none;
  font-size: var(--font-size02);
  height: var(--height-buttons-selects);
}

.select01{
  width: 340px;
  background-color: var(--cor02);
}

main {
  position: relative;
  top: 250px;
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
}

tr:hover {
  color:var(--cor02);
  background-color: var(--cor06);
}

td,
th {
  padding: 5px;
}

.ver-mais img {
  width: 15px;
}
</style>
