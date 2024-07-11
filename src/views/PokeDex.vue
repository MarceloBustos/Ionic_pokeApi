<template>
    <ion-page>
        <ion-header>
            <ion-toolbar color="primary">
                <ion-title>Pokemon</ion-title>
            </ion-toolbar>
        </ion-header>

        <ion-content>
            <ion-grid>
                <ion-row>
                    <ion-searchbar class="ion-no-padding ion-padding-top" color="ligth"
                        placeholder="Escriba una cantidad" fill="solid" type="number" v-model="cantidad">

                    </ion-searchbar>
                    <ion-button color="dark" size="small" expand="full" shape="round" @click="getData">
                        Mostrar Pokemon
                    </ion-button>
                </ion-row>
                <div class="ion-padding" v-for="(pokemon, index) in pokemonArray" :key="index">
                    <!-- Card para pokemon -->

                    <ion-card :style="{ backgroundColor: pokemon.colorCard }">
                        <ion-card-content>
                            <ion-row>
                                <ion-col size="5" class="contPokemon">
                                    <ion-img :src="pokemon.sprites.other.dream_world.front_default"
                                        class="pokemon"></ion-img>
                                </ion-col>

                                <ion-col size="7" class="contData">
                                    <ion-card-subtitle class="numero">Id: #{{ pokemon.id }}</ion-card-subtitle>
                                    <ion-card-title>Nombre: {{ pokemon.name }}</ion-card-title>
                                    <ion-card-subtitle>
                                        <ion-badge><ion-icon :icon="pokemon.icono"></ion-icon>{{
                                            pokemon.types[0].type.name
                                        }}</ion-badge>
                                    </ion-card-subtitle>
                                </ion-col>
                            </ion-row>
                        </ion-card-content>
                    </ion-card>
                </div>
            </ion-grid>
        </ion-content>
    </ion-page>
</template>

<script>
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonGrid, IonCard, IonCardHeader, IonCardContent, IonRow, IonCol, IonImg, IonCardTitle, IonCardSubtitle, IonBadge, IonIcon, IonSearchbar, IonButton } from '@ionic/vue';
import { flash, water, leaf, flame, warning, medal, paperPlane } from 'ionicons/icons';
export default {
    name: 'PokeDex',
    components: {
        IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonGrid, IonCard, IonCardHeader, IonCardContent, IonRow, IonCol, IonImg, IonCardTitle, IonCardSubtitle, IonBadge, IonIcon, IonSearchbar, IonButton
    },
    data() {
        return {
            flash, water, leaf, flame, warning, medal, paperPlane,
            //arreglo para almacenar pokemons
            pokemonArray: [],
            //arreglo para almacenar pokemons id
            pokemonId: 1,
            // cantidad de pokemos a buscar
            cantidad: 10,
            //maximo de pokemons
            maxId: 807,
        }
    },
    methods: {
        async getData() {

            try {
                //Reiniciar mi arreglo de pokemons
                this.pokemonArray = [];

                for (let i = 0; i < this.cantidad; i++) {
                    //obtener el id del pokemon
                    this.pokemonId = Math.floor(Math.random() * this.maxId) + 1;

                    try {
                        //Realizar solicitud de información
                        const response = await fetch(`https://pokeapi.co/api/v2/pokemon/${this.pokemonId}`);

                        //Verificar respuesta exitosa
                        if (!response.ok) {
                            throw new Error('No se pudo obtener la data');
                        }

                        //Convertir la respuesta a Json
                        const data = await response.json();

                        if (!data.sprites.other.dream_world.front_default) {
                            i--;
                        } else {
                            switch (data.types[0].type.name) {
                                case 'dragon':
                                    data.colorCard = '#7038F8';
                                    data.icono = this.paperPlane;
                                    //Asignar el pokemon al arrya
                                    this.pokemonArray.push(data);
                                    break;
                                case 'fighting':
                                    data.colorCard = '#F08030';
                                    data.icono = this.medal;
                                    //Asignar el pokemon al arrya
                                    this.pokemonArray.push(data);
                                    break;
                                case 'fire':
                                    data.colorCard = '#C03028';
                                    data.icono = this.flame;
                                    //Asignar el pokemon al arrya
                                    this.pokemonArray.push(data);
                                    break;
                                case 'water':
                                    data.colorCard = '#6890F0';
                                    data.icono = this.water;
                                    //Asignar el pokemon al arrya
                                    this.pokemonArray.push(data);
                                    break;
                                case 'grass':
                                    data.colorCard = '#78C850';
                                    data.icono = this.leaf;
                                    //Asignar el pokemon al arrya
                                    this.pokemonArray.push(data);
                                    break;
                                case 'electric':
                                    data.colorCard = '#F8D030';
                                    data.icono = this.flash;
                                    //Asignar el pokemon al arrya
                                    this.pokemonArray.push(data);
                                    break;
                                case 'psychic':
                                    data.colorCard = '#F55099';
                                    data.icono = this.warning;
                                    //Asignar el pokemon al arrya
                                    this.pokemonArray.push(data);
                                    break;
                                default:
                                    i--;
                                    break;
                            }

                        }

                        //log de la data

                    } catch (error) {
                        console.log(error);
                    }
                }
                console.log(this.pokemonArray);
            } catch (error) {
                console.log(error);
            }
        }
    },
    mounted() {
        this.getData();
    }
}
</script>

<style>
.pokemon {
    width: 100px;
    position: relative;
    height: 200px;
    margin: auto;
    max-width: 200px;
}

ion-card {
    border-radius: 8px;
    margin: 3% 0%;
}

ion-card-content {
    display: contents;
}

.contPokemon {
    background: rgba(0, 0, 0, 0.3);
    border-radius: 50% 50% 50% 0%;
}

.contData {
    justify-content: center;
    margin: auto;
    padding-left: 10%;
}

ion-badge {
    margin-top: 10px;
    background: rgba(0, 0, 0, 0.3);
}

ion-card-title {
    font-weight: 900;
}

.numero {
    color: rgba(0, 0, 0, 0.6);
    font-weight: bold;
}

ion-button {
    padding-top: 10px;
    margin: auto;
}
</style>