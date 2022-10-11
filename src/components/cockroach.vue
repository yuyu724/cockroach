<template>
    <div class="container">
        <div class="TopNav">
            <h2>^-^</h2>
        </div>
        <div class="top">
            <div class="block" @click="Select(item, index, $event)" v-for="(item, index) in allblock" :key="index"
                :style="{left: item.left, top: item.top}">
                <!-- Select(item, index, $event) -->
                <div class="block_main">
                    <img :src="item.src" alt=""
                        style="width:45px; height: 45px; border-radius: 10px; border: 2px solid white;">
                </div>


            </div>
        </div>
        <div class="foot">
            <div class="select" v-for="(item, index) in selectblock" :key="index">

                <img :src="item.src" alt=""
                    style="width:45px; height: 45px; border-radius: 10px; border: 2px solid white;">
            </div>

        </div>

        <div class="btn">
            <svg class="icon" aria-hidden="true" @click="reserve">
                <use xlink:href="#icon-refresh"></use>
            </svg>

        </div>
    </div>

    <!-- <div>
        赢了之后显示的模块
    </div> -->
</template>




<script>
import cunqiang from '../image/cunqiang.jpg'
import jiang from '../image/jiang.jpg'
import xiaoming from '../image/xiaoming.jpg'
import xiaofeng from '../image/xiaofeng.jpg'
import haliluya from '../image/haliluya.jpg'
import haitun from '../image/haitun.jpg'
import jun from '../image/jun.jpg'
import alien from '../image/alien.jpg'
import hong from '../image/hong.jpg'
import qiang from '../image/qiang.jpg'
import sha from '../image/sha.jpg'
import zhitian from '../image/zhitian.jpg'



export default {
    data() {
        return {
            default: true,
            allblock: [], //全部图片块
            selectblock: [],//选中的图片块
            currentblock: {}, //当前选中待消除的模块
            positionblock: [], //数组定位
            picList: [
                { picIndex: 0, pic: cunqiang },
                { picIndex: 1, pic: jiang },
                { picIndex: 2, pic: xiaoming },
                { picIndex: 3, pic: xiaofeng },
                { picIndex: 4, pic: haliluya },
                { picIndex: 5, pic: haitun },
                { picIndex: 6, pic: jun },
                { picIndex: 7, pic: hong },
                { picIndex: 8, pic: qiang },
                { picIndex: 9, pic: sha },
                { picIndex: 10, pic: zhitian },

            ]
        }
    },
    created() {
        this.InitData();
    },
    methods: {
        //函数部分

        Select(item, index, e) {
            let that = this;
            //console.log(item, index, e);
            console.log(e);
            console.log(e.srcElement);

            //JudgeCover方法里运用ownerDocument获取目标属性返回节点所属的根元素（Document 对象）
            //所以这里要传入的参数是e.srcElement
            let Cover = that.JudgeCover(e.srcElement);
            if (!Cover) { //如果没有覆盖
                that.allblock.splice(index, 1);
                that.selectblock.push(item);
                that.currentblock = item; //将选中的图片块的item赋值给currentblock，便于之后做判断
                //console.log(e);
                console.log(that.currentblock);

                //差一个排序，即按照选中栏中的图片块的index进行排序
                that.selectblock.sort((a, b) => {
                    return a.imgindex - b.imgindex
                })

                //放入一个时钟，延后做判断
                setTimeout(() => {
                    this.checkSelect();
                    //console.log(selectblock);
                    if (that.selectblock.length >= 6) {
                        alert("垃圾^-^")
                        that.InitData();
                    }
                    if (that.allblock.length == 0) {
                        //全部图片块数组清空，即玩家已经全部消除
                        // that.default = true
                    }
                }, 200)
            }



        },


        //三连重复判断（判断是否可以消除）
        checkSelect() {
            let num = 0;
            let that = this;
            that.selectblock.forEach(item => {
                if (that.currentblock.src == item.src) {
                    num++;
                }
            })
            if (num == 3) {
                that.selectblock = that.selectblock.filter(item => {
                    return that.currentblock.src != item.src
                })
            }
        },


        //判断是否被遮盖
        JudgeCover(element) {
            //这里获取属性返回节点所属的根元素是为了后续利用elementFromPoint(x, y)获取节点在全DOM的坐标
            let document = element.ownerDocument;
            console.log(document);
            //经典取目标元素的坐标方法getBoundingClientRect:
            let res = element.getBoundingClientRect();
            console.log(res);
            let x = res.x;
            let y = res.y;
            //console.log(x);
            let width = res.width;
            let height = res.height;
            x |= 0; //等价于 x= x|0 ,基础知识按位与，只有当x = 0时结果才为0
            y |= 0;
            width |= 0;
            height |= 0;

            //四点取样
            let elements = [
                document.elementFromPoint(x + 1, y + 1),
                document.elementFromPoint(x + width - 1, y + 1),
                document.elementFromPoint(x + 1, y + height - 1),
                document.elementFromPoint(x + width - 1, y + height - 1)
            ];

            return elements.filter(
                (ele) => ele !== null
            )
                .some(
                    (el) => el !== element && !element.contains(el)
                )
        },

        InitData() {//  初始化数据
            let that = this;
            that.positionblock = [];
            that.allblock = [];
            that.selectblock = [];
            that.currentblock = {};

            let index = 0
            //第一层
            let left = 0;
            let top = 0;

            for (let i = 0; i < 9; i++) {
                for (let j = 0; j < 6; j++) {
                    that.positionblock.push({ id: index++, imgindex: 1, src: cunqiang, left: left + 'px', top: top + 'px' });
                    left += 50;
                }
                left = 0;
                top += 50;
            }

            //第二层
            left = 25;
            top = 25;
            for (let i = 0; i < 8; i++) {
                for (let j = 0; j < 5; j++) {
                    that.positionblock.push({ id: index++, imgindex: 1, src: xiaoming, left: left + 'px', top: top + 'px' });
                    left += 50;
                }
                left = 25;
                top += 50
            }
            //console.log(allblock);
            that.CreateRandomImg();
        },

        //生成随机图标
        CreateRandomImg() {
            let index = 0;
            //先清空全部图块的数组，再生成，不然会重叠乱套
            let that = this;
            let random = 0;
            that.allblock = [];
            for (let i = 0; i <= 99; i++) {
                if (i % 3 == 0) { //再增加一层对数量的判断
                    random = parseInt(Math.random() * 11)

                    //不能把allblock.push写在if循环里，不然就会在第一个i%30=0的时候就停止添加
                }
                that.allblock.push({
                    id: index++,
                    src: that.picList[random].pic,
                    imgindex: that.picList[random].picIndex
                })
            }
            that.allblock.sort(() => {
                return Math.random() > 0.5 ? -1 : 1;
            })
            that.allblock.forEach((item, index) => {
                //console.log(item.id);
                item.id = index;
            })


            let pos = 0
            that.allblock.forEach(item => {
                item['left'] = that.positionblock[pos].left
                item['top'] = that.positionblock[pos].top
                pos++
                if (pos > 72) {
                    pos = 0
                }
            })

            console.log(that.allblock);

        },

        reserve(e) {
            console.log(e);
            this.InitData()
        }




    }
}

//变量定义

</script>











<style scoped lang="less">
.container {
    margin: 0 auto;
    padding: 0;
    width: 320px;
    height: 700px;
    border-radius: 5px;
    position: relative;
    border: 4px solid black;
    background-color: rgba(247, 223, 85, 0.671);

    .TopNav {
        height: 50px;
        border: 1px solid black;
        font-size: large;
    }

    .top {
        width: 100%;
        height: 500px;
        position: relative;

        .block {
            width: 47px;
            height: 47px;
            line-height: 50px;
            text-align: center;
            position: absolute;
            text-align: center;


            .block_main {
                margin-top: 20px;
                margin-left: 10px;
                border-radius: 10px;
            }
        }
    }

    .foot {
        width: 320px;
        height: 70px;
        margin: 0 auto;
        border-radius: 5px;
        border: 4px solid black;
        margin-left: -4px;
        background-color: rgba(247, 223, 85, 0.671);

        .select {
            margin-top: 10px;
            display: inline-block;

        }
    }

    .btn {
        .icon {
            width: 40px;
            height: 40px;
            padding-top: 15px;
        }
    }
}
</style>
