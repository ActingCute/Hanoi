<template>
    <div class="box">

        层数：<input type="number" v-model="num" />
        <div class="game-box">
            <div class="game" @drop="drop" @dragover.prevent>
                <div class="g-item-box" :style={width:w+px}>
                    <div class="g-item">
                        <div class="drop-field">
                            <div :data-num="i" @dragend="dragend" @dragstart="dragstart(i,1)" draggable="true"
                                :key="i+'num1'" v-for="i in num1">
                                {{i}}
                            </div>
                        </div>
                    </div>
                </div>
                <div class="g-item-box" :style={width:w+px}>
                    <div class="g-item">
                        <div class="drop-field">
                            <div :data-num="i" @dragend="dragend" @dragstart="dragstart(i,2)" draggable="true"
                                :key="i+'num2'" v-for="i in num2">
                                {{i}}
                            </div>
                        </div>
                    </div>
                </div>
                <div class="g-item-box" :style={width:w+px}>
                    <div class="g-item">
                        <div class="drop-field">
                            <div :data-num="i" @dragend="dragend" @dragstart="dragstart(i,3)" draggable="true"
                                :key="i+'num3'" v-for="i in num3">
                                {{i}}
                            </div>
                        </div>
                    </div>
                </div>
            </div>

        </div>
    </div>
</template>

<script>
    export default {
        name: 'Game',
        computed: {},
        data() {
            return {
                W: 0,
                px: "px",
                num: 3,
                min_setup: 0,
                w: "0",
                droppedItem: "",
                ax: 0,
                bx: 0,
                cx: 0,
                num1: [],
                num2: [],
                num3: [],
                old_space: 1
            }
        },
        mounted() {
            this.W = document.body.clientWidth;
            this.w = this.W / 3;
            this.ax = this.w;
            this.init();
        },

        watch: {
            num(old) {
                if (old < 3) {
                    alert("最小是3")
                    this.num = 3;
                } else {
                    this.init();
                }
            }
        },

        methods: {
            init() {
                this.min_setup = 0;
                this.num1 = [];
                this.num2 = [];
                this.num3 = [];

                this.hanoi(this.num, "A", "B", "C");
                for (let i = 0; i < this.num; i++) {
                    this.num1.push(i + 1)
                }
                console.log("min_setup:", this.min_setup)
            },
            drop(event) {
                console.log("drop");
            },
            dragstart(item, old_space) {
                this.droppedItem = item
                this.old_space = old_space
            },
            dragend(event) {
                event.dataTransfer.clearData()
                let x = event.x
                //判断是在第几个区域
                if (x < this.w) {
                    //第一
                    if (this.canMove(this.droppedItem, this.num1)) {
                        let old_nums = this.getOldNums()
                        old_nums.splice(old_nums.indexOf(this.droppedItem), 1);
                        this.num1.push(this.droppedItem)
                        this.num1.sort()
                    }
                    //console.log("第一", this.droppedItem, this.canMove(this.droppedItem, this.num1))
                } else if (x < this.w * 2) {
                    if (this.canMove(this.droppedItem, this.num2)) {
                        let old_nums = this.getOldNums()
                        old_nums.splice(old_nums.indexOf(this.droppedItem), 1);
                        this.num2.push(this.droppedItem)
                        this.num2.sort()
                    }
                    //console.log("第二", this.droppedItem, this.canMove(this.droppedItem, this.num2))
                } else {
                    if (this.canMove(this.droppedItem, this.num3)) {
                        let old_nums = this.getOldNums()
                        old_nums.splice(old_nums.indexOf(this.droppedItem), 1);
                        this.num3.push(this.droppedItem)
                        this.num3.sort()
                    }
                    //console.log("第三", this.droppedItem, this.canMove(this.droppedItem, this.num3))
                }
                if (this.num3.length == this.num) {
                    alert("恭喜通关")
                }
            },
            dragover(obj) {
                console.log("dragover - ", obj);
            },
            hanoi(n, a, b, c) {
                this.min_setup++;
                if (n == 1) {
                    console.log("Move " + n + " from " + a + " to " + c);
                } else {
                    this.hanoi(n - 1, a, c, b);
                    console.log("Move " + n + " from " + a + " to " + c)
                    this.hanoi(n - 1, b, a, c);
                }
            },
            canMove(num, nums) {
                if (nums.indexOf(num) == -1) {
                    //判断是最小的一个数
                    //新的区域的最小数
                    for (let i = 0; i < nums; i++) {
                        if (num > nums[i]) {
                            console.log("new 不是最小的数，不能移动")
                            return false
                        }
                    }
                    //旧的区域的最小数
                    var old_nums = this.getOldNums()
                    for (let i = 0; i < old_nums.length; i++) {
                        console.log("num -- ", num, "old_nums[i]", old_nums[i], num > old_nums[i])
                        if (num > old_nums[i]) {
                            console.log("old 不是最小的数，不能移动")
                            return false
                        }
                    }
                    return true
                }
                return false
            },
            getOldNums() {
                if (this.old_space == 1) {
                    return this.num1
                } else if (this.old_space == 2) {
                    return this.num2
                } else {
                    return this.num3
                }
            }
        },
    }
</script>

<style lang="less">
    .box {
        text-align: center;
        margin: 0 auto;

        .game-box {
            text-align: center;

            .game {
                .g-item-box {

                    display: inline-block;

                    .g-item {
                        width: 50%;
                        border-bottom: 1px #00ff00 solid;
                    }
                }

                text-align: center;
                position: relative;
            }

            position: absolute;
            bottom: 10%;
        }
    }
</style>