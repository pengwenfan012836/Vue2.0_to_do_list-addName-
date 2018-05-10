<template>
    <div>
        <table class="table">
            <thead>
                <tr>
                    <td v-for="title in titles"
                    @click="sort(title)">{{ title | capitalize }}</td>
                </tr>
            </thead>
            <tbody>
                <tr v-for="row in countData">
                    <td v-for="col in row">{{ col }}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>
<script>
export default {
    name: 'myGrid',
    props: {
        titles: Array,
        tableDatas: Array,
        searchQuery: String
    },
    data () {
        var sortWay = {};
        for (var i =0; i< this.titles.length; i++) {
            sortWay[this.titles[i]] = 1;
        }
        return {
            sortKey: '',
            sortWay: sortWay
        }
    },
    computed: {
        countData: function() {
            var data = this.tableDatas;
            var searchQuery = this.searchQuery;
            if (searchQuery) {
                var temData = [];
                for (var i = data.length-1; i >= 0; i--) {
                    for (var j in data[i]) {
                        if ((data[i][j] +'').includes(searchQuery)) {
                            temData.push(data[i]);
                            break;
                        }
                    }
                }
                return temData;
            }
            /**
             * 当这里引入是一个对象/数组的时候，
             * 那会产生实时监听的效果么？不会
             * @type {[type]}
             */
            var sortKey = this.sortKey;
            var sortWay = this.sortWay[sortKey] || 1;
            if (sortKey) {
                data = data.slice().sort(function (a, b) {
                    a = a[sortKey]
                    b = b[sortKey]
                    return (a === b ? 0 : a > b ? 1 : -1) * sortWay
                })
            }
            return data;
        }
    },
    filters: {
        capitalize: function(value) {
            return value.charAt(0).toUpperCase() + value.slice(1);
        }
    },
    methods: {
        sort: function(key) {
            this.sortKey = key;
            this.sortWay[key] = this.sortWay[key] * -1;
        }

    }
}
</script>
<style>
    .table {
        border:1px solid #999;
        width: 500px;
    }
    .inputClass {
        margin-bottom: 20px;
    }
</style>
