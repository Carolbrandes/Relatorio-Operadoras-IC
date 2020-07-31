<template>
  <header class="header">
   <Titulo texto="Relatório de Operadoras" tituloPrincipal />
  </header>
</template>

<script>
import Titulo from './Title';

export default {
  components:{Titulo}  
};
</script>

<style>
.header {
  padding: 1% 0;
  margin-bottom: 50px;
}
</style>