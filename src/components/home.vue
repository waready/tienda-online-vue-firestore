<template>
    <v-jumbotron color="blue" lighten="2">
        <v-container fill-height>
            <v-layout align-center>
                <v-flex>
                    <h3 class="display-3">
                        {{ $t('welcome.title',{app: 'Vuejs 2 + Friestore'} ) }}
                    </h3>
                    <span class="subheading">
                        sistema de control de recurencias en trabajos y 
                        tareas realizados en el colegio de ingenieros por 
                        los practicantes de area
                    </span>
                    <v-divider class="my-3"/>
                    <div class="title mb-3">
                        INGRESA TUS TAREAS
                    </div>
                    <v-btn class="mx-0" color="primary" to="/login" large>
                        ENTRAR
                    </v-btn>
                </v-flex>
            </v-layout>
        </v-container>
    </v-jumbotron>    
</template>

<script>
export default {
    
}
</script>

<style>
    
</style>

