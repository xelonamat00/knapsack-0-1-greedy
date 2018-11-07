<template>
    <div class="container">
        <h4>Knapsack Problem With Greedy Algorithm</h4>
        <!-- {{inits.length}} -->
        <div class="row">
            <!-- <form @submit="addData()" class="form-inline">
               <div class="form-row">
                    <div class="col-auto">
                        <div class="form-group col-md-7">
                        Weight: <input type="text" v-model="weight" class="form-control form-control-sm">
                        </div>
                    </div>
                    <div class="col-auto">
                        <div class="form-group col-md-7">
                            Profit: <input type="text" v-model="profit" class="form-control form-control-sm">
                        </div>
                    </div>
                    <div class="col-auto">
                        <div class="form-group col-sm-2">
                            <input type="hidden" v-model="sumDensity" class="form-control form-control-sm">
                        </div>
                    </div>
                    <input type="submit" class="btn btn-primary">
               </div>
            </form>
            <table class="table table-sm mt-3">
                <thead>
                    <tr>
                        <th>Weight</th>
                        <th>Height</th>
                        <th>Density</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(init, index) in inits" :key="index">
                        <td>{{init.weight}}</td>
                        <td>{{init.profit}}</td>
                        <td>{{init.density}}</td>
                    </tr>
                </tbody>
            </table> -->
            <div class="row">
                <form class="form-inline" @submit="greedyByDensity()">
                    <div class="col">
                        <div class="form-group">
                            <input type="text" placeholder="Maximum Capacity" v-model="maxCapacity">
                        </div>
                    </div>
                    <button type="submit" class="btn btn-success">Calculate</button>
                </form>
            </div>
            <table class="table mt-3">
                <thead>
                    <tr>
                        <th>Weight</th>
                        <th>Profit</th>
                        <th>Density</th>
                        <th>Status</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(init, index) in inits" :key="index">
                        <td>{{init.weight}}</td>
                        <td>{{init.profit}}</td>
                        <td>{{init.density}}</td>
                        <td>{{init.status}}</td>
                    </tr>
                </tbody>
            </table>
            <!-- greedy by weight -->
            <div class="row">
                <div class="col-6">
                    <form class="form-inline">
                        <div class="form-group">
                            <label for="profit" class="text text-danger">Total Profit: </label>
                            <input type="text" id="profit" v-model="totalProfit" class="form-control">
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data () {
            return {
                // parameter
                weight: null,
                profit: null,
                density: null,
                status: 0,

                inits: [
                    {weight: 23, profit: 67, density: 2.913043478, status: 0},
                    {weight: 45, profit: 86, density: 1.911111111, status: 0},
                    {weight: 67, profit: 139, density: 2.074626866, status: 0},
                    {weight: 39 , profit: 42 , density: 1.076923077, status: 0}
                ],
                initsAsc: [],
                isSorted: false,
                totalBobot: 0,
                maxCapacity: null,
                available: true,
                x: [],
                totalProfit: 0,
                i: 0
            }
        },
        methods: {
            addData () {
                if (!this.checkForm) {
                    return
                }
                this.inits.push({
                    weight: parseInt(this.weight),
                    profit: parseInt(this.profit),
                    density: parseFloat(this.density),
                    status: this.status
                })
            },
            sortAscendent () {
               function compare (a, b) {
                   if (a.weight < b.weight) {
                       return -1
                   }
                   if (a.weight > b.weight) {
                       return 1
                   }
                   return 0
               }
               this.isSorted = true
                // this.initsAsc = JSON.parse(JSON.stringify(this.inits))
                this.initsAsc = Array.from(this.inits)
            //    console.log(this.initsAsc)
               return this.initsAsc.sort(compare)
            },
            sortDescendent () {
                function compare(a, b) {
                    if(a.profit < b.profit) {
                        return 1
                    }
                    if(a.profit > b.profit) {
                        return -1
                    }
                    return 0
                }
                this.isSorted = true
                this.initsAsc = Array.from(this.inits)
                return this.initsAsc.sort(compare)
            },
            sortDescendentDensity () {
                function compare(a, b) {
                    if(a.density < b.density) {
                        return 1
                    }
                    if(a.density > b.density) {
                        return -1
                    }
                    return 0
                }
                this.isSorted = true
                this.initsAsc = Array.from(this.inits)
                return this.initsAsc.sort(compare)
            },
            greedyByWeight () {
                this.sortAscendent()
                // var i = 0
                // console.log(this.inits.length)
                // console.log(this.initsAsc.length)
                // if(JSON.stringify(this.initsAsc[i]) === JSON.stringify(this.inits[i])){
                //     this.initsAsc[i].status = 1
                //     console.log(this.inits)
                //     console.log(this.initsAsc)
                //     alert('success')
                // }
                var i = 0
                while(i < this.inits.length){
                    if(this.totalBobot + this.initsAsc[i].weight <= this.maxCapacity){
                        for(var a = 0; a < this.inits.length; a++){
                            if(JSON.stringify(this.initsAsc[i]) === JSON.stringify(this.inits[a])){
                                this.inits[a].status = 1
                                this.totalProfit += this.inits[a].profit
                                this.totalBobot += parseInt(this.initsAsc[i].weight)
                                console.log(this.totalBobot)
                                break
                            }
                        }
                    }
                    i++
                }
            },
            greedyByProfit () {
                this.sortDescendent()
                var i = 0
                while(i < this.inits.length){
                    if(this.totalBobot + this.initsAsc[i].weight <= this.maxCapacity){
                        for(var a = 0; a < this.inits.length; a++){
                            if(JSON.stringify(this.initsAsc[i]) === JSON.stringify(this.inits[a])){
                                this.inits[a].status = 1
                                this.totalProfit += this.inits[a].profit
                                this.totalBobot += parseInt(this.initsAsc[i].weight)
                                console.log(this.totalBobot)
                                break
                            }
                        }
                    }
                    i++
                }
            },
            greedyByDensity () {
                this.sortDescendentDensity()
                var i = 0
                while(i < this.inits.length){
                    if(this.totalBobot + this.initsAsc[i].weight <= this.maxCapacity){
                        for(var a = 0; a < this.inits.length; a++){
                            if(JSON.stringify(this.initsAsc[i]) === JSON.stringify(this.inits[a])){
                                this.inits[a].status = 1
                                this.totalProfit += this.inits[a].profit
                                this.totalBobot += parseInt(this.initsAsc[i].weight)
                                console.log(this.totalBobot)
                                break
                            }
                        }
                    }
                    i++
                }
            }
        },
        computed: {
            sumDensity () {
                return this.density = this.profit / this.weight
            },
            checkForm () {
                return this.weight !== null && this.profit !== null
            },
            
            // updateObj () {
            //     this.sortAscendent()
            //     while(this.i < this.inits.length){
            //         if(this.totalBobot + parseInt(this.initsAsc[this.i].weight) <= this.maxCapacity){
            //             for(var a = 0; a < this.inits.length; a++){
            //                 if(JSON.stringify(parseInt(this.initsAsc[this.i])) === JSON.stringify(parseInt(this.inits[a]))){
            //                     this.inits[a].status = 1
            //                     this.totalBobot += parseInt(this.initsAsc[this.i].weight)
            //                     this.i++
            //                     break
            //                 }
            //             }
            //         }
            //     }
            // }
        }
    }
</script>