<template>
    <div class="home">

        <div class="row">
            <div class="list-container col-md-4">

                <div class="add-location-container">
                    <form>
                        <div class="form-group">
                            <p>Долгота</p>
                            <input v-model="longitude" type="text" />
                        </div>

                        <div class="form-group">
                            <p>Широта</p>
                            <input v-model="latitude" type="text" />
                        </div>

                        <div class="form-group">
                            <p>Название</p>
                            <input v-model="name" type="text" />
                        </div>

                        <div class="form-group">
                            <input type="submit" v-on:click="add" value="Добавить"/>
                        </div>
                    </form>
                    
                </div>

                <table class="table">
                    <thead>
                        <tr>
                            <th scope="col">Название</th>
                            <th scope="col">Долгота</th>
                            <th scope="col">Широта</th>
                            <th scope="col"></th>
                            <th scope="col"></th>
                        </tr>
                    </thead>
                    <tr v-for="mark in marks">
                        <td v-on:click="movemap("+ {{ mark.name }} + ")" scope="col">{{ mark.name }} </td>
                        <td scope="col">{{ mark.longitude }} </td>
                        <td scope="col">{{ mark.latitude }} </td>
                        <td scope="col">
                            <button class="btn btn-primary">Редактировать</button>
                        </td>
                        <td scope="col">
                            <button class="btn btn-danger">Удалить</button>
                        </td>
                    </tr>
                </table>


            </div>

            <div class="map-container col-md-8" >
                <yandex-map v-for="mark in marks" :coords="coords" style="height:600px">
                    <ymap-marker
                        marker-id={{ mark.name }}
                        :coords ="[mark.longitude , mark.latitude]">
                    </ymap-marker>
                </yandex-map>
            </div>
        </div>

    </div>
</template>

<script>
    import { yandexMap, ymapMarker } from 'vue-yandex-maps'
    import { loadYmap } from 'vue-yandex-maps'
    const axios = require('axios').default;


    export default {
        components: { yandexMap, ymapMarker, loadYmap },

         data: () => ({
             coords: [54, 39],
             longitude: null,
             latitude: null,
             name: null,
             marks: []
        }),

         methods: {
            add: function () {
                axios
                    .post("/add", {
                        params: {
                            Name: this.name,
                            Longitude: this.longitude,
                            Latitude: this.latitude
                        }
                    })
                    .then(this.marks.push({
                        name: this.name,
                        longitude: this.longitude,
                        latitude: this.latitude
                    }))
             },

             remove: function () {
                 axios
                     .delete("/delete")
                     .then()
             },

             movemap: function (name) {
                 var pos = marks.find(elem => elem.name == name);
                 this.coords = [pos.longitude, pos.latitude];
             }
        },

        mounted() {
            loadYmap();

            axios
                .get("/get-marks")
                .then(responce => this.marks = responce)
        }

    }
</script>

<style scoped>
</style>