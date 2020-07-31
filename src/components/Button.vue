<template>
  <button :class="{button, todos}">{{texto}}</button>
</template>

<script>
export default {
    props:{
        texto: {},
        button: {type: Boolean},
        todos: {type: Boolean}
    }
}
</script>

<style>
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
 
</style>