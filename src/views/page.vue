<template>
    <div class="page">
        <div class="table">
            <div class="col" v-for="(col,index) in data" :key="index">
                <div @click="addPiece(cell.type, index, index2)" class="cell" v-for="(cell, index2) in col" :key="index2">{{cell.type == 'o'? 'O' : (cell.type == 'x' ? 'X' : '')}} <span style="opacity: 0;">1</span> </div>
            </div>
        </div>
		
		<div class="box">
			<p class="black" v-text="islast?('胜者'+succ):flag?'现在是x的回合':'现在是o的回合'">现在是{{flag?'x':'o'}}的回合</p>
			<p @click="backTo(0)">开始</p>
		<p v-for="item in times" @click="backTo(item)">第{{item}}步</p>
		</div>
		
    </div>
</template>
<script>
export default {
    data() {
        return {
            data: [
                [{type: ''}, {type: ''}, {type: ''}],
                [{type: ''}, {type: ''}, {type: ''}],
                [{type: ''}, {type: ''}, {type: ''}]
            ],
			step:[],
			times:0,
			islast:false,
			succ:'',
            flag: false,  // false奇数画圆，true偶数画叉
        }
    },
	watch:{
		'succ':function(nv,ov){
			console.log(nv)
			if(nv==''){
				this.islast=false
			}else{
				this.islast=true
			}
		}
	},
    methods: {
		backTo(mytimes){
			this.succ=''
			if(mytimes%2==0){
				this.flag=false;
				
			}else{
				this.flag=true;
			}
			this.data=JSON.parse(JSON.stringify(this.step[mytimes]));;
			this.times=mytimes;
			this.step=this.step.slice(0,mytimes+1);
			console.log(mytimes)
			console.log('step:')
			console.log(this.step)
			console.log('data:')
			console.log(this.data)
		},
        addPiece(type, colIndex, cellIndex) {
			if(this.islast==true||this.times==9){
				return
			}
            if (!this.flag) {   // 画圆s
                this.data[colIndex][cellIndex].type = 'o'
				
            } else {
                this.data[colIndex][cellIndex].type = 'x'
				
				
            }
			
            this.flag = !this.flag;
			
			this.times++;
			 var mydata=JSON.parse(JSON.stringify(this.data));
			
			 // this.step.push(mydata);
			this.step.push(mydata)
			console.log(this.step)
			console.log(this.times)
            // 行
            if ((this.data[colIndex][0].type == this.data[colIndex][1].type) && (this.data[colIndex][1].type == this.data[colIndex][2].type)) {
                if (this.data[colIndex][0].type == 'o') {
                   this.succ='o'
                   //alert("恭喜o胜利！")
					
                } else if (this.data[colIndex][0].type == 'x') {
                  this.succ='x'
                  //alert("恭喜x胜利！")
					
                }
            }
            // 列
            if ((this.data[0][cellIndex].type == this.data[1][cellIndex].type) && (this.data[1][cellIndex].type == this.data[2][cellIndex].type)) {
                if (this.data[0][cellIndex].type == 'o') {
                   this.succ='o'
                   //alert("恭喜o胜利！")
					
                } else if (this.data[0][cellIndex].type == 'x') {
                   this.succ='x'
                   //alert("恭喜x胜利！")
                }
            }
            // 对角
            if ((this.data[0][0].type == this.data[1][1].type) && (this.data[1][1].type == this.data[2][2].type)) {
                if (this.data[0][0].type == 'o') {
                   this.succ='o'
                   //alert("恭喜o胜利！")
                } else if (this.data[0][0].type == 'x') {
                   this.succ='x'
                   //alert("恭喜x胜利！")
                }
            }
            if ((this.data[0][2].type == this.data[1][1].type) && (this.data[1][1].type == this.data[2][0].type)) {
                if (this.data[0][2].type == 'o') {
					this.succ='o'
                    //alert("恭喜o胜利！")
                } else if (this.data[0][2].type == 'x') {
					this.succ='x'
                    //alert("恭喜x胜利！")
                }
            }
        },
		first(){
			var mydata1=JSON.parse(JSON.stringify(this.data));
		
		 // this.step.push(mydata);
		this.step.push(mydata1)
		console.log(this.step)
		}
		
    },
	created(){
		this.first()
	}
}
</script>
<style>
    .page{
        padding: 0;
        margin: 0;
        text-align: center;
    }
    .table{
        width: 450px;
        height: 450px;
        margin: 50px auto;
    }
    .col{
        height: 150px;
        width: 100%;
        white-space: nowrap;
    }
    .cell{
        width: 150px;
        height: 150px;
        display: inline-block;
        line-height: 150px;
        border: 2px solid #999;
		font-size: 30px;
		padding-left:40px ;
		text-align: center;
    }
	.box{
		position: absolute;
		top: 50px;
		left: 50px;
		width: 200px;
		
		text-align: center;
	}
	.box p{
		font-size: 16px;
		border-bottom: 1px solid #2C3E50;
		cursor: pointer;
	}
	.black{
		font-weight: 600;
		font-size: 18px;
		cursor: default;
	}
</style>