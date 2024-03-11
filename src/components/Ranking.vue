<template>
    <div>
        <v-select class="selecter" @update:modelValue="select_table()" :items="title" v-model="select" item-title="name"
            item-value="str" return-object>
        </v-select>
        <table>
            <tr>
                <th>順位</th>
                <th>名前</th>
                <th>得点</th>
            </tr>
            <tr v-for="(data, index) in table.data">
                <td>{{ currentPage * 10 + parseInt(index) + 1 }}</td>
                <td>{{ data.name }}</td>
                <td>{{ data.point }}</td>
            </tr>
        </table>
        <v-pagination :length="lastPage"></v-pagination>
    </div>
</template>
<script>
export default {
    data() {
        return {
            title: [
                { name: 'スロット', str: 'slot' },
                { name: 'ブロック崩し', str: 'blockbreaker' },
            ],
            now_title: '',
            select: { name: 'スロット', str: 'slot' },
            table: Object,
            currentPage: 0,
            lastPage: 0,
        }
    },
    mounted() {
        this.get_table()
    },
    methods: {
        select_table() {
            setTimeout(this.get_table, 1)
        },
        async get_table() {
            const url = 'http://54.199.63.195/api/' + this.select['str'] + '/index';
            // 指定したURLからデータを取得
            const response = await fetch(url);
            // データをJavaScriptのオブジェクトに変換
            const data = await response.json();
            this.table = data;
            this.lastPage = this.table.last_page
        },

    }

}
</script>
<style scoped>
h2 {
    margin: 40px;
    text-align: center;
}

table {
    margin: 0 auto;
    width: 600px;
    font-size: 30px;
}

table,
th,
td {
    border: 1px solid green;
    color: green;
}

td {
    text-align: center;
}

.selecter {
    width: 200px;
}
</style>