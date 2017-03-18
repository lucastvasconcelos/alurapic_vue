<template>
    <div id="app">
        <h1 class="centralizando">{{msg}}</h1>
        <input class="filtro" type="search" @input="filtro = $event.target.value" placeholder="Filtre pelo titulo">
        <ul class="lista-fotos">
            <li v-for="foto of filtrarPorTitulo" class="lista-fotos-item">
                <meu-painel :titulo="foto.titulo">
                    <imagem-responsiva :url="foto.url" :alt="foto.titulo"> </imagem-responsiva>
                </meu-painel>
            </li>
        </ul>
    </div>
</template>

<script>
    import Painel from '../shared/painel/Painel.vue';
    import ImagemResponsiva from '../shared/imagem-responsiva/ImagemResponsiva.vue';
    
    export default {
        components: {
            'meu-painel': Painel,
            'imagem-responsiva': ImagemResponsiva
        },
        name: 'app',
        data() {
            return {
                msg: 'AluraPic',
                fotos: [],
                filtro: "",
            }
        },
        computed: {
            filtrarPorTitulo() {
                if (this.filtro) {
                    let exp = new RegExp(this.filtro.trim(), 'i');
                    return this.fotos.filter(foto => exp.test(foto.titulo));
                } else {
                    return this.fotos;
                }
            }
        },
        created() {
            this.$http.get('http://localhost:3000/v1/fotos')
                .then(res => res.json())
                .then(res => this.fotos = res, err => console.log(err));
        }
    }
</script>

<style>
    .centralizando {
        text-align: center;
    }
    
    .lista-fotos {
        list-style: none;
    }
    
    .lista-fotos .lista-fotos-item {
        display: inline-block;
    }
    
    .filtro {
        display: block;
        width: 100%;
    }
</style>