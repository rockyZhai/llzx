<template>
	<div class="content1"
		v-loading.fullscreen.lock="fullscreenLoading">
		<el-row :gutter="12" class="one_card">
			<div style="text-align: left">您所管辖的出租房户数：10户，房间数：23间</div>
			<el-col :span="5">
				<el-card style="background-color: #c9c9c9" shadow="hover">
					<el-rate
					v-model="value0"
					disabled
					text-color="#ff9900">
					</el-rate>
					<div class="starH">
						<!-- <img src="../style/img/home.svg" alt=""> -->
						<span>1户</span>
					</div>
				</el-card>
			</el-col>
			<el-col :span="5">
				<el-card style="background-color: #7ec368" shadow="hover">
					<el-rate
					v-model="value1"
					disabled>
					</el-rate>
					<div class="starH">
						<!-- <img src="../style/img/home.svg" alt=""> -->
						<span>2户</span>
					</div>
				</el-card>
			</el-col>
			<el-col  :span="5">
				<el-card style="background-color: #ad6df6" shadow="hover">
					<el-rate
					v-model="value3"
					disabled>
					</el-rate>
					<div class="starH">
						<!-- <img src="../style/img/home.svg" alt=""> -->
						<span>2户</span>
					</div>
				</el-card>
			</el-col>
			<el-col :span="5">
				<el-card style="background-color: #eb6a42" shadow="hover">
					<el-rate
					v-model="value5"
					disabled
					>
					</el-rate>
					<div class="starH">
						<!-- <img src="../style/img/home.svg" alt=""> -->
						<span>5户</span>
					</div>
				</el-card>
			</el-col>
		</el-row>
		<div class="echars">
			<div id="myChart" style="width: 1200px; height: 600px;"></div>
			<div id="myChart2" style="width: 800px; height: 650px;"></div>
		</div>
	</div>
</template>  
  
<script>  
	// var moment = require('moment');
export default {  
  name: 'home',  
  data () {  
    return {  
		fullscreenLoading: false,	
		value5: 5,
		value3: 3,
		value0: 0,
		value1: 1,
    	}  
  	},
  	computed: {
      
    },
  	created(){
    	// this.list()
    },
    
  	methods: {
      	
    },
    components: {
	
	},
	mounted() {
		let myChart = this.$echarts.init(document.getElementById('myChart'));
		let myChart2 = this.$echarts.init(document.getElementById('myChart2'));
		myChart.setOption({
			title: { text: '各星级房屋所占比重' },
            tooltip : {
				trigger: 'axis',
				axisPointer : {            // 坐标轴指示器，坐标轴触发有效
					type : 'shadow'        // 默认为直线，可选为：'line' | 'shadow'
				}
			},
			legend: {
				data:['0星出租房','1星出租房','3星出租房','5星出租房','出租房总数']
			},
			grid: {
				left: '3%',
				right: '4%',
				bottom: '3%',
				containLabel: true
			},
			xAxis : [
				{
					type : 'category',
					data : [ "屏峰社区","杨家牌楼社区","东岳社区","茶市街社区","留下社区","石马社区","金鱼井社区"]
				}
			],
			yAxis : [
				{
					type : 'value'
				}
			],
			series : [
				{
					name:'0星出租房',
					type:'bar',
					data:[20, 32, 35, 24, 39, 30, 40]
				},
				{
					name:'1星出租房',
					type:'bar',
					stack: '广告',
					data:[20, 32, 35, 24, 40, 30, 40]
				},
				{
					name:'3星出租房',
					type:'bar',
					stack: '广告',
					data:[20, 12, 13, 24, 20, 30, 30]
				},
				{
					name:'5星出租房',
					type:'bar',
					stack: '广告',
					data:[50, 36, 37, 34, 50, 50, 50]
				},
				{
					name:'出租房总数',
					type:'bar',
					data:[110, 112, 120, 120, 124, 160, 170],
					markLine : {
						lineStyle: {
							normal: {
								type: 'dashed'
							}
						},
						data : [
							[{type : 'min'}, {type : 'max'}]
						]
					}
				},
			]
        });
		myChart2.setOption({
			backgroundColor: '#2c343c',

    title: {
        text: '辖区内各星级房屋所占比重图',
        left: 'center',
        top: 20,
        textStyle: {
			color: '#ccc',
			fontsize:'2rem'
        }
    },

    tooltip : {
        trigger: 'item',
		formatter: "{a} <br/>{b} : {c} ({d}%)",
		
    },

    visualMap: {
        show: false,
        min: 80,
        max: 600,
        inRange: {
            colorLightness: [0, 1]
		},
		textStyle: {
			fontsize:'2rem'
        }
    },
    series : [
        {
            name:'访问来源',
            type:'pie',
            radius : '55%',
            center: ['50%', '50%'],
            data:[
                {value:335, name:'1星出租房'},
                {value:310, name:'2星出租房'},
                {value:274, name:'3星出租房'},
                {value:235, name:'4星出租房'},
                {value:400, name:'5星出租房'}
            ].sort(function (a, b) { return a.value - b.value; }),
            roseType: 'radius',
            label: {
                normal: {
                    textStyle: {
						color: 'rgba(255, 255, 255, 0.3)',
						fontsize:'2rem'
                    }
                }
            },
            labelLine: {
                normal: {
                    lineStyle: {
                        color: 'rgba(255, 255, 255, 0.3)'
                    },
                    smooth: 0.2,
                    length: 10,
					length2: 20,
					textStyle: {
						fontsize:'2rem'
					}
				},
				textStyle: {
					fontsize:'2rem'
				}
            },
            itemStyle: {
                normal: {
                    color: '#c23531',
                    shadowBlur: 200,
                    shadowColor: 'rgba(0, 0, 0, 0.5)'
                }
            },

            animationType: 'scale',
            animationEasing: 'elasticOut',
            animationDelay: function (idx) {
                return Math.random() * 200;
            }
        }
    ]
		})
	}
}  
</script>  

<style>
	.one_card {
		width: 95%;
		height: 180px;
		display: inline-block;
		background-color: #ffffff;
		margin: 20px auto;
		padding-left: 32px;
		line-height: 64px;
	}
	.one_card .el-col {
		display: inline-block;
		height: 80px;
	}
	.content1 .el-card {
		height: 5rem;
	}
	.one_card .el-card__body {
		padding: 0;
		text-align: center;
  	}
	.el-rate {
		margin-top: .625rem
	}
	.starH {
		width: 5rem;
		height: 32px;
		position: relative;
		text-align: center;
		margin: 10px auto;
	}
	.starH > img {
		width: 2rem;
		height: 2rem;
		position: absolute;
		left: 0;
	}
	.starH > span {
		display: inline-block;
		width: 32px;
		height: 32px;
		line-height: 2.5rem;
		position: absolute;
		left: 2.1875rem;
		top: 0;
		color: #ffffff;
	}
	.echars {
		margin-top: 3rem;
		background-color: #fff;
		padding-top: 3.125rem;
		width: 95%;
		margin: 30px auto;
	}
	.echars > #myChart {
		display: inline-block;
	}
	#myChart2 {
		display: inline-block;
		margin-left: 8.5rem;
		margin-top: 4.625rem;
	}
</style>

