<template>
    <div class="container">
    
        <select class="custom-select form-control mb-3" @change="e=>selectedTable=e.target.value">
            <option selected>Choose table</option>
            <option value="1">MainSwaps (all swaps on block level)</option>
            <option value="2">Contracts</option>
        </select>

        <div v-if="selectedTable == 1">
            <div>
                <div class="input-group input-group-sm mb-3">
                    <div class="input-group-prepend">
                        <span class="input-group-text" id="inputGroup-sizing-sm">Blocks</span>
                    </div>
                    <input type="text"  @change="e=>MainSwapsDetails.blocks=e.target.value" placeholder="1 block = 12 seconds. default is 300" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm">
                </div>
                <div class="input-group input-group-sm mb-3">
                    <div class="input-group-prepend">
                        <span class="input-group-text" id="inputGroup-sizing-sm">Buy ratio</span>
                    </div>
                    <input type="text" @change="e=>MainSwapsDetails.buyRatio=e.target.value" placeholder="leave blank if not filtering. value between 0 and 1" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm">
                </div>
                <div class="input-group input-group-sm mb-3">
                    <div class="input-group-prepend">
                        <span class="input-group-text" id="inputGroup-sizing-sm">Volume greater than</span>
                    </div>
                    <input type="text" @change="e=>MainSwapsDetails.volume=e.target.value" placeholder="leave blank if not filtering" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm">
                </div>
                <div class="input-group input-group-sm mb-3">
                    <div class="input-group-prepend">
                        <span class="input-group-text" id="inputGroup-sizing-sm">Age less than (minutes)</span>
                    </div>
                    <input type="text" @change="e=>MainSwapsDetails.age=e.target.value" placeholder="leave blank if not filtering" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm">
                </div>
                <div class="input-group input-group-sm mb-3">
                    <div class="input-group-prepend">
                        <span class="input-group-text" id="inputGroup-sizing-sm">Total buys greater than</span>
                    </div>
                    <input type="text" @change="e=>MainSwapsDetails.totalBuys=e.target.value" placeholder="leave blank if not filtering" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm">
                </div>
                <div class="input-group input-group-sm mb-3">
                    <div class="input-group-prepend">
                        <span class="input-group-text" id="inputGroup-sizing-sm">Marketcap less than</span>
                    </div>
                    <input type="text"  @change="e=>MainSwapsDetails.marketCap=e.target.value" placeholder="leave blank if not filtering" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm">
                </div>
            </div>
            <div class="d-flex justify-content-center">
                <button type="button" class="btn btn-primary"  @click="()=>handleSearch()" >{{ querying ? `Querying...` : `Go` }}</button>
            </div>
        </div>
        <div v-if="selectedTable == 2">
            <select class="custom-select form-control mb-3" v-if="selectedTable==2" @change="e=>{
                selectedContractsTable= e.target.value;
                ContractDetails.table = e.target.value;
            }">
                <option selected>Choose table</option>
                <option value="m1">Contracts1m</option>
                <option value="m2">Contracts5m</option>
                <option value="m3">Contracts15m</option>
                <option value="m4">Contracts1h</option>
                <option value="m5">Contracts1d (not functional yet)</option>
            </select>
            <div v-if="selectedContractsTable">
                <div>
                    <div class="input-group input-group-sm mb-3">
                        <div class="input-group-prepend">
                            <span class="input-group-text" id="inputGroup-sizing-sm">Sort by contract</span>
                        </div>
                        <input type="text"  @change="e=>ContractDetails.contract=e.target.value" placeholder="put contract here" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm">
                    </div>
                    <div class="input-group input-group-sm mb-3">
                        <div class="input-group-prepend">
                            <span class="input-group-text" id="inputGroup-sizing-sm">Timeframe</span>
                        </div>
                        <input type="text" @change="e=>ContractDetails.timespan=e.target.value" placeholder="default= last 1,5,15,etc mins." class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm">
                    </div>
                    <!-- <div class="input-group input-group-sm mb-3">
                        <div class="input-group-prepend">
                            <span class="input-group-text" id="inputGroup-sizing-sm">Buy ratio</span>
                        </div>
                        <input type="text" placeholder="only use if not filtering by contract" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm">
                    </div>
                    <div class="input-group input-group-sm mb-3">
                        <div class="input-group-prepend">
                            <span class="input-group-text" id="inputGroup-sizing-sm">Volume greater than</span>
                        </div>
                        <input type="text" placeholder="only use if not filtering by contract" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm">
                    </div>
                    <div class="input-group input-group-sm mb-3">
                        <div class="input-group-prepend">
                            <span class="input-group-text" id="inputGroup-sizing-sm">Age less than (minutes)</span>
                        </div>
                        <input type="text" placeholder="only use if not filtering by contract" class="form-control" aria-label="Small" aria-describedby="inputGroup-sizing-sm">
                    </div> -->
                </div>
                <div class="d-flex justify-content-center">
                    <button type="button" class="btn btn-primary"  @click="()=>handleSearch()" >{{ querying ? `Querying...` : `Go` }}</button>
                </div>
            </div>


        </div>

        <!-- <select class="custom-select form-control mb-3" v-if="selectedTable==2">
            <option selected>Choose table</option>
            <option value="1">MainSwaps (all swaps on block level)</option>
            <option value="2">Contracts</option>
        </select> -->
        <!-- <div class="input-group">
            <select class="custom-select" id="inputGroupSelect04">
                <option selected>Choose...</option>
                <option value="1">One</option>
                <option value="2">Two</option>
                <option value="3">Three</option>
            </select>
            <div class="input-group-append">
                <button class="btn btn-outline-secondary" type="button">Button</button>
            </div>
        </div> -->


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
enum ContractsTable {
            m1="1m",
            m5="5m",
            m15="15m",
            h1="1h",
            d1="1d"
        }
export default defineComponent({
    name: 'Search',
    setup() {
        const text = ref("");
        const ip = '127.0.0.1';
        const isLoading = ref(false);
        const data = ref([]);
        const selectedTable = ref(0)
        const selectedContractsTable = ref(0)
        const querying = ref(false)
        const MainSwapsDetails = ref({
            blocks: 300,
            buyRatio: 0,
            volume: 1000,
            age: 0,
            totalBuys: 0,
            marketCap: 100000
        })
        const ContractDetails = ref({
            table: ContractsTable.m1,
            contract: "",
            timespan: 25


        })


        function handleEnter () {
            return '';
        };
        async function handleSearch() {
            querying.value = true;
            setTimeout(()=>querying.value = false, 1000);
            if (selectedTable.value == 1) {
                data.value = await getMainSwaps();
                //const query = `SELECT * from MainSwaps where `
            } else if (selectedTable.value == 2) {

            }
        }

        async function getMainSwaps() {
            try {
                const response = await axios.get(`/api/swaps/grouped?blocks=${MainSwapsDetails.value.blocks}&buyRatio=${MainSwapsDetails.value.buyRatio}&marketCap=${MainSwapsDetails.value.marketCap}&totalBuys=${MainSwapsDetails.value.totalBuys}&volume=${MainSwapsDetails.value.volume}&age=${MainSwapsDetails.value.age}`)
                return response.data.data;
            } catch(e){
                console.log(e)
            }
        }

        async function callApi() {
            try {
                const response = await axios.get(`/api/sql?sqlText=${text.value}`)
            } catch(e){
                console.log(e)
            }
        }

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
        return { handleEnter, handleSearch, isLoading, data, callApiArbitrarySql, text, selectedTable, selectedContractsTable, querying, ContractsTable,MainSwapsDetails, ContractDetails }
    }
})

</script>


<style>

</style>