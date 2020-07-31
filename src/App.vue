<template>
  <div id="app">
    <div class="fixed">
      <header class="header">
        <h1 class="titulo-principal">Relatório de Operadoras</h1>
      </header>

      <div class="container filtros">
        <button @click="getOperadoras" class="button todos">Todas</button>

        <select name="modalidade" id="modalidade">
          <option>Por Modalidade</option>
           <option v-for="modalidade in getModalidades" :key="modalidade" value="modalidade">{{modalidade}}</option> 
        </select>

        <select name="cidade" id="cidade">
          <option>Por Cidade</option>
          <option v-for="cidade in getCidades" :key="cidade" value="cidade">{{cidade}}</option>  
        </select>

        <select name="estado" id="estado">
          <option>Por Estado</option>
          <option v-for="estado in getEstados" :key="estado" value="estado">{{estado}}</option>
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

          <tbody>
            <tr v-for="(operadora, index) in operadoras" :key="operadora.CNPJ">
              <td>{{index+1}}</td>
              <td>{{operadora["Razão Social"]}}</td>
              <td>{{operadora["CNPJ"]}}</td>
              <td>{{operadora["Modalidade"]}}</td>
              <td>{{operadora["Cidade"]}}</td>
              <td>{{operadora["UF"]}}</td>
              <td>
                <a class="ver-mais" href="#"><img src="/images/ver-mais.svg" alt="ver mais"></a>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  data: () => {
    return {
      operadoras: {},
      cidades: null
    };
  },

  methods: {
    getOperadoras() {
      fetch("/data/cadop.json")
        .then((resp) => resp.json())
        .then((resp) => (this.operadoras = resp));
    },

    filtrarCampos(chaveCampo){
       let campo = Array.from(this.operadoras);
      campo = campo.map(operadora => operadora[chaveCampo]);
      campo = Object.values(campo);

      let novaLista = [];

      campo.forEach(c => {
        if(!novaLista.includes(c)){
          novaLista.push(c)
        }
      })

      return novaLista.sort();
    }
  },

  computed:{
    getCidades(){
      return this.filtrarCampos("Cidade");
    },

     getEstados(){
      return this.filtrarCampos("UF");
    },

    getModalidades(){
      return this.filtrarCampos("Modalidade");
    }

  },

  mounted() {
    this.getOperadoras()
  },
};
</script>

<style>
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
  background-color: #131313;
  z-index: 2;
}

.header {
  padding: 1% 0;
  margin-bottom: 50px;
}

.titulo-principal {
  color: #ffffff;
  text-align: center;
  font-size: 1.5em;
  text-transform: uppercase;
  font-weight: 400;
}

.filtros {
  margin-bottom: 50px;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
}

.button,
select {
  border-radius: 5px;
  padding: 5px 10px;
  outline: none;
  border: none;
  cursor: pointer;
  font-size: 1.1em;
}

.button.todos,
select {
  display: block;
  background-color: #f0ecec;
  color: #131212;
  width: 150px;
  height: 40px;
  text-align: center;
}

select {
  padding-left: 10px;
  width: 200px;
}

main{
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

tr, thead{
  border: 1px solid #919090;
}

tr{
  color: #555454;
  background-color: #e9e9e9;
  font-size: 0.8em;
  transition: background-color 1s ease-in-out, color 1.2s ease-in-out
}

tr:hover{
  color: #fff;
  background-color: #706f6f;
}

td, th{
  padding: 5px;
}

td{
  border: 1px solid #919090;
  height: 20px;
}

th{
  background-color: #a19f9f;
  color: #fff;
  font-size: 0.9em;
}

.ver-mais img{
  width: 15px;
}

</style>
