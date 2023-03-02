<template>
    <div class="container">
    
    <div class="input-group">
        <input class="form-control" placeholder="sql query" @input="(e)=>text=e.target.value" :value="text" @keypress.enter="(e)=>handleEnter()"/>
        <button class="btn btn-outline-secondary" type="button"  @click="()=>{handleSearch(); callApiArbitrarySql(text);}">{{isLoading ? 'Searching... ' : ' Search '}}</button>
    </div>
    <table class="table table-bordered table-striped" v-if="data.length">
        <thead>
            <tr>
            
            <th scope="col">#</th>
            <th scope="col">First</th>
            <th scope="col">Last</th>
            <th scope="col">Handle</th>
            </tr>
        </thead>
        <tbody>
            <tr>
            <th scope="row">1</th>
            <td>Mark</td>
            <td>Otto</td>
            <td>@mdo</td>
            </tr>
            <tr>
            <th scope="row">2</th>
            <td>Jacob</td>
            <td>Thornton</td>
            <td>@fat</td>
            </tr>
            <tr>
            <th scope="row">3</th>
            <td colspan="2">Larry the Bird</td>
            <td>@twitter</td>
            </tr>
        </tbody>
    </table>
    </div>
</template>



<script lang="ts">
import { ref, defineComponent } from "vue";
import axios from 'axios'
const is_local = true;

export default defineComponent({
    name: 'SearchAdvanced',
    setup() {
        const text = ref("");
        const ip = '127.0.0.1';
        const isLoading = ref(false);
        const data = ref([]);
        function handleEnter () {
            return '';
        };
        function handleSearch() {
            isLoading.value = true;
            setTimeout(()=>isLoading.value = false, 5000);
        }

        // async function callApi() {
        //     try {
        //         const response = await axios.get(`/api/sql?sqlText=${text.value}`)
        //     } catch(e){
        //         console.log(e)
        //     }
        // }

        async function callApiArbitrarySql(sqlText: string) {
            const forbiddenWords = ['drop', 'delete', 'alter', 'update' ]
            for (let i in forbiddenWords) {
                if (sqlText.search(forbiddenWords[i]) > 0) {
                    console.log('lafkds', forbiddenWords[i])
                    return;
                }
            }
            sqlText = sqlText.replace("+","%2B")
            console.log(sqlText)
            try {
                const response = await axios.get(`/api/sql?sqlText="${sqlText}"`)
            } catch(e){
                console.log(e)
            }

        }
        return { handleEnter, handleSearch, isLoading, data, callApiArbitrarySql, text }
    }
})

</script>


<style>

</style>