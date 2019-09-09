<template>
    <div class="hello">
        <form>
            <p>количество комнат:</p>
            <select multiple="multiple" name="rooms" v-model="rooms">
                <option value="item.rooms" v-for="item of selectRooms">{{ item.rooms }}</option>
            </select>
            <hr>
            <p>цена:</p>
            <input type="range" :min="selectPrice.min" :max="selectPrice.max" placeholder="price" name="price"
                   v-model="price"/>
            {{this.price}}
            <hr>
            <input type="text" value="" placeholder="этажность" name="floor" v-model="floor"/>
            <input type="checkbox" value="1" name="notlast" v-model="notlast"/> <label>not first not last</label>
            <hr>
            <p>год сдачи:</p>
            <select multiple="multiple" name="years" v-model="years">
                <option value="item.year" v-for="item of selectYears">{{ item.year }}</option>
            </select>
            <hr>
            <button @click.prevent="getParams">подобрать</button>
        </form>

        <hr>
        <div v-for="apartment of searchedApp" :key="'app' + apartment.id">
            <p>id: {{apartment.id}}
                || name: {{apartment.name}}
                || rooms: {{apartment.rooms}}
                || price: {{apartment.price}}
                || этаж: {{apartment.floor}}
                || этажность дома: {{apartment.max}}
                || год сдачи: {{apartment.year}}</p>
            <hr>
        </div>

    </div>
</template>

<script>
    export default {
        name: 'HelloWorld',
        props: {
            msg: String
        },
        data() {
            return {
                rooms: [],
                price: this.centerPrice,
                floor: '',
                notlast: false,
                years: [],
                params: [],
                apartments: [
                    {
                        'id': 1,
                        'name': 'соколовая 21',
                        'rooms': '3',
                        'price': '1000000',
                        'floor': 5,
                        'max': 25,
                        'year': 2020
                    },
                    {
                        'id': 2,
                        'name': 'московская 2',
                        'rooms': '1',
                        'price': '5000000',
                        'floor': 3,
                        'max': 9,
                        'year': 2019
                    },
                    {
                        'id': 3,
                        'name': 'советская 95',
                        'rooms': '4',
                        'price': '509000',
                        'floor': 9,
                        'max': 9,
                        'year': 2021
                    },
                    {
                        'id': 4,
                        'name': 'рахова 23',
                        'rooms': '2',
                        'price': '309000',
                        'floor': 1,
                        'max': 5,
                        'year': 2023
                    },
                    {
                        'id': 5,
                        'name': 'кирова 89',
                        'rooms': '7',
                        'price': '2509000',
                        'floor': 7,
                        'max': 9,
                        'year': 2021
                    },
                    {
                        'id': 6,
                        'name': 'кирова 8',
                        'rooms': '2',
                        'price': '1509000',
                        'floor': 9,
                        'max': 9,
                        'year': 2024
                    },
                    {
                        'id': 7,
                        'name': 'кутякова 12',
                        'rooms': '4',
                        'price': '12509000',
                        'floor': 1,
                        'max': 12,
                        'year': 2019
                    },
                    {
                        'id': 8,
                        'name': 'садовая 2',
                        'rooms': '2',
                        'price': '12509000',
                        'floor': 3,
                        'max': 12,
                        'year': 2025
                    }
                ]
            }
        },
        computed: {
            centerPrice() {
                return (this.selectPrice / 2)
            },
            selectRooms() {
                return this.getLists('rooms');
            },
            selectYears() {
                return this.getLists('year');
            },
            selectPrice() {
                let prices = this.apartments.map((item) => {
                    return parseInt(item.price)
                });
                return {'min': Math.min.apply(null, prices), 'max': Math.max.apply(null, prices)}
            },
            searchedApp() {
                let apps = this.apartments;
                if (this.params.length > 0) {
                    this.params.map((item, index, array) => {
                        if (index === 0 && (array[index].length > 0)) {
                            item.map((room, i, rooms) => {
                                apps = apps.filter(p => {
                                    return parseInt(p.rooms) === parseInt(room)
                                });
                            })
                        } else if (index === 1) {
                            apps = apps.filter(p => {
                                return parseInt(p.price) <= parseInt(item);
                            });
                        } else if (index === 2 && (array[index] !== '')) {

                            apps = apps.filter(p => {
                                if (array[3]) {
                                    if (parseInt(p.floor) > 1 && parseInt(p.floor) < p.max) {
                                        return parseInt(p.floor) === parseInt(item);
                                    } else {
                                        return false
                                    }
                                } else {
                                    return parseInt(p.floor) === parseInt(item);
                                }
                            });
                        } else if (index === 4 && (array[index].length > 0)) {
                            item.map((year, i, years) => {
                                apps = apps.filter(p => {
                                    return parseInt(p.year) === parseInt(year)
                                });
                            })
                        }

                    })
                }
                return apps
            }
        },
        methods: {
            getLists(param) {
                return this.apartments.map((item) => {
                    return {[param]: item[param]}
                }).filter((obj, pos, arr) => {
                    return arr.map(mapObj => mapObj[param]).indexOf(obj[param]) === pos
                }).sort((a, b) => parseInt(a[param]) - parseInt(b[param]))
            },
            getParams() {
                this.params = [
                    this.rooms,
                    this.price,
                    this.floor,
                    this.notlast,
                    this.years
                ];
            }
        },
        mounted() {
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
    h3 {
        margin: 40px 0 0;
    }

    ul {
        list-style-type: none;
        padding: 0;
    }

    li {
        display: inline-block;
        margin: 0 10px;
    }

    a {
        color: #42b983;
    }
</style>
