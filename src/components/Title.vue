<template>
   <h1 :class="{tituloPrincipal}">{{texto}}</h1>
</template>

<script>
export default {
    props:{
        texto: {},
        tituloPrincipal: {type: Boolean}
    }
}
</script>

<style>

.tituloPrincipal {
  color: var(--cor02);
  text-align: center;
  font-size: var(--font-size03);
  text-transform: uppercase;
  font-weight: 400;
}

</style>