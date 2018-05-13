<template>
    <div>

        <div style="text-align: center">
            <h1 style="text-transform: capitalize">{{this.type + ' overview:'}}</h1>

            <div>
                <button @click.prevent="setType('departure')">Departures</button>
                <button @click.prevent="setType('arrival')">Arrivals</button>
            </div>

            <br><br>

            <div style="display: inline-flex">
                <input placeholder="search" v-model="searchTerm"/>
                <datepicker placeholder="date" v-model="date"></datepicker>
                <select v-model="time">
                    <option v-for="option in times">{{option}}</option>
                </select>
                <button @click.prevent="loadData">Search</button>
            </div>

            <br><br>
            {{url}}

        </div>


        <br><br>

        <div v-if="loading">
            <span>Loading...</span>
        </div>

        <div v-else-if="entries.length == 0">
            <span>No results</span>
        </div>

        <entries-table v-else :entries="entries" :type="type"></entries-table>
    </div>


</template>
<script>

    import axios from 'axios';
    import moment from 'moment'
    import Datepicker from 'vuejs-datepicker';
    import EntriesTable from './entries-table.vue';


    export default {

        components: {
            Datepicker,
            EntriesTable
        },

        data() {

            return {

                loading: true,

                entries: [],

                times: ['00:00', '01:00', '02:00', '03.00', '04:00', '05:00', '06:00', '07:00', '08:00', '09:00', '10:00', '11:00', '12:00', '13:00', '14:00', '15:00', '16:00', '17:00', '18:00', '19:00' , '20:00', '21:00', '22:00', '23:00'],
                time: '00:00',
                date: '',
                urlBase: 'https://planeapi.herokuapp.com/',
//                urlBase: 'http://planeapi.local/',
                type: 'departure',
                searchTerm: '',
            }
        },

        computed: {

            url()
            {
                let url = this.urlBase;

                url += this.type;
                url += '?query=' + this.searchTerm;
                url += '&date=' + this.formatDate;
                url += '&time=' + this.formatTime;

                return url;
            },
//
            formatDate()
            {
                if (this.date === '') return this.date;

                return moment(this.date).format('DD') + ' - ' + moment(this.date).format('MM') + ' - ' + moment(this.date).format('YYYY');
            },

            formatTime()
            {
                return this.time.substr(0, 2);
            }
        },

        methods: {

            setType(type)
            {
                this.type = type;
                this.loadData();
            },

            loadData()
            {
                this.loading = true;

                axios.get(this.url).then(function(response) {

                    console.log(response);
                    this.entries = response.data;
                    this.loading = false;

                }.bind(this));
            },
        },


        created() {

            this.loadData();

//            axios
//                .get('https://api.coindesk.com/v1/bpi/currentprice.json')
//                .then(response => (console.log(response)))

        }

    }
</script>
<style scoped>

    table {
        width: 100%;
        text-align: left;
    }

</style>
