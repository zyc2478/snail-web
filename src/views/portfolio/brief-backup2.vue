<style type="text/css">
    .test{
        margin: 20px 20px 20px 20px;     
    }
    .test Button{
        margin-bottom: 10px;
    }
</style>
<!--<template>
    <div class="test">
        <Button type="warning" @click="click()">test</Button>
        <Button type="warning" @click="click2()">测试</Button>
        <Input v-model="value" type="textarea" :rows="4" placeholder="Enter something..."></Input>
    </div>
</template>
<script>
/*    import Drilldown from '../node_modules/highcharts/modules/Drilldown.js'
    import Highcharts from 'highcharts'
    Drilldown(Highcharts);

    <vue-highcharts :highcharts="Highcharts" :options="drilldownOptions" ref="drilldownChart"></vue-highcharts>*/

    export default {
        data(){
            return {
                value: null
            }
        },
        methods: {
            click(){
                this.$store.dispatch('userLogin',{"user_name":"test1","user_password":"123","router":this.$router});
                this.$router.push({ path: 'base' }) ;
            },
            click2(){
                this.axios({
                    /*headers: {'Authorization': 'bearer '+this.$store.state.users.currentUser.UserToken},*/
                    method: 'post',
                    url: '/test',
                    data: {
                        "test": "123456"
                    }
                }).then(function(response){
                    /*console.log(response);*/
                    this.value = response.data;
                }.bind(this)).catch(function(error){
                    console.log(error);
                });
            }
        }   
    };
</script>-->
<template>
    <div>
        <vue-highcharts :options="options" ref="lineCharts"></vue-highcharts>
        <button @click="load">load</button>
    </div>
</template>

<script>
    import VueHighcharts from 'vue2-highcharts'
    //Test code start
    function requestData() {
        $.ajax({
            url: 'http://localhost:8080/bbd_ds/vwBidList/getVwBidList/30',
            success: function(point) {
            },
            cache: false
        });
    }

    //Test code end

/*    const asyncData = {
        name: 'Tokyo',
        marker: {
            symbol: 'square'
        },
        data: [7.0, 6.9, 9.5, 14.5, 18.2, 21.5, 25.2, {
            y: 26.5,
            marker: {
                symbol: 'url(http://www.highcharts.com/demo/gfx/sun.png)'
            }
        }, 23.3, 18.3, 13.9, 9.6]
    }*/

/*
    var chart = null; // 定义全局变量
    $(document).ready(function() {
        chart = Highcharts.chart('container', {
            chart: {
                type: 'spline',
                events: {
                    load: requestData // 图表加载完毕后执行的回调函数
                }
            },
            title: {
                text: 'Live random data'
            },
            xAxis: {
                type: 'datetime',
                tickPixelInterval: 150,
                maxZoom: 20 * 1000
            },
            yAxis: {
                minPadding: 0.2,
                maxPadding: 0.2,
                title: {
                    text: 'Value',
                    margin: 80
                }
            },
            series: [{
                name: '随机数据',
                data: []
            }]
        });
    });
*/

    export default{
        components: {
            VueHighcharts
        },
        data(){
            return{
                options: {
                    chart: {
                        type: 'spline',
                    },
                    title: {
                        text: 'Live random data'
                    },
                    xAxis: {
                        type: 'datetime',
                        tickPixelInterval: 150,
                        maxZoom: 20 * 1000
                    },
                    yAxis: {
                        minPadding: 0.2,
                        maxPadding: 0.2,
                        title: {
                            text: 'Value',
                            margin: 80
                        }
                    },
                    series: [{
                        name: '随机数据',
                        data: []
                    }]
                }
            }
        },
        methods: {
/*            load(){
                let lineCharts = this.$refs.lineCharts;
                lineCharts.delegateMethod('showLoading', 'Loading...');
/!*                setTimeout(() => {
                    lineCharts.addSeries(requestData());
                    lineCharts.hideLoading();
                }, 2000);*!/
                this.getList();
            },*/
            load(){
                let lineCharts = this.$refs.
                var vm = this.$http.get('http://localhost:8080/bbd_ds/vwBidList/getVwBidList/30',{})
                    .then(function (response) {
                        vm.json = response.data
                        lineCharts.chart('container', {
                            chart: {
                                zoomType: 'x'
                            },
                            title: {
                                text: 'bidCount Over Time'
                            },
                            subtitle: {
                                text: document.ontouchstart === undefined ?
                                    'Click and drag in the plot area to zoom in' : 'Pinch the chart to zoom in'
                            },
                            xAxis: {
                                type: 'datetime'
                            },
                            yAxis: {
                                title: {
                                    text: 'bidCount'
                                }
                            },
                            legend: {
                                enabled: false
                            },
                            plotOptions: {
                                area: {
                                    fillColor: {
                                        linearGradient: {
                                            x1: 0,
                                            y1: 0,
                                            x2: 0,
                                            y2: 1
                                        },
                                        stops: [
                                            [0, lineCharts.getOptions().colors[0]],
                                            [1, lineCharts.Color(lineCharts.getOptions().colors[0]).setOpacity(0).get('rgba')]
                                        ]
                                    },
                                    marker: {
                                        radius: 2
                                    },
                                    lineWidth: 1,
                                    states: {
                                        hover: {
                                            lineWidth: 1
                                        }
                                    },
                                    threshold: null
                                }
                            },

 /*                           series: [{
                                type: 'area',
                                name: 'Sessions',
                                data: vm.json.map(d => [new Date(d.date).getTime(), d.sessions])
                            }]*/
                            series: [{
                                type: 'area',
                                name: 'Sessions',
                                data: vm.json
                                    .sort((a,b) => new Date(a.bid_date) - new Date(b.bid_date))
                                    .map(d => [new Date(d.bid_date).getTime(), d.bid_count])
                            }]
                        });
                    });
            }

        }
    }
</script>