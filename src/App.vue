<template>
    <div id="app">
      <LazyLoading :items="list"/>
    </div>
</template>

<script>
    import LazyLoading from './components/LazyLoading';

    export default {
        name: 'App',
        LIST_COUNT: 1000,
        components: {
            LazyLoading
        },
        data: () => {
            return {
                list: []
            }
        },
        created(){
            for(let i =0; i < this.$options.LIST_COUNT; i++){
                this.list.push( 'item number ' + (i +1));
            }
        }
    }
</script>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Architects+Daughter&display=swap');
    html, body {
        font-family: 'Architects Daughter', cursive;
        padding: 0;
        margin: 0;
        height: 100%;
        min-height: 100%;
    }
    #app {

        font-size: 16px;
        height: 100%;
        min-height: 100%;
    }
</style>
