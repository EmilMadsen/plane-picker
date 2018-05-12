<template>

        <table v-else>
            <tr>
                <th>Time</th>
                <th>Expected</th>
                <th>Company</th>
                <th>{{ type == 'departure' ?  'Destination' : 'From'}}</th>
                <th>Gate</th>
                <th>Terminal</th>
                <th>Status</th>
            </tr>

            <tr v-for="entry in entries">
                <td>{{entry.time}}</td>
                <td>{{entry.expected}}</td>
                <td>{{entry.company}}</td>
                <td>{{entry.destination}}</td>
                <td>{{entry.gate}}</td>
                <td>{{entry.terminal}}</td>
                <td>{{entry.status}}</td>
            </tr>
        </table>

</template>
<script>

    export default {


        data() {
            return {
            }
        },

        props: ['entries', 'type'],

    }
</script>
<style scoped>

    table {
        width: 100%;
        text-align: left;
    }

</style>