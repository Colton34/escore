<template>

    <div v-if="item.header" class="question" id="question" :ref="eachrefs">
        <div style="text-align: center"><a style= "font-size: 25px">{{item.header.name}}</a></div>
<a >
        <div class="infodiv">
            <div style="float: left; margin: 10px 0px 0px 10px"> 姓名：________________</div>
            <div style="float: left; margin: 0px 0px 0px 10px"> 学校：________________</div>
            <div style="float: left; margin: 0px 0px 0px 10px"> 学号：________________</div>
            <div style="float: left; margin: 0px 0px 0px 10px"> 缺考：[是] [否]</div>
            <div style="float: left; margin: 5px 0px 0px 10px"> barcode：<div style="border: 1px #1f2d3d solid;width: 160px;height: 40px;"></div></div>
        </div>
        <div class="stunum"><div>

            <div class="stunumheader" style="border: 0px solid #000;width: 100%; text-align: left;margin-left: 2px">
                学号：
            </div>
            <table class="stunumbody" style=";margin-left: 25px">
                <tr><td></td><td>  </td><td>[0]</td><td>[0]</td><td>[0]</td><td>[0]</td><td>[0]</td><td>[0]</td><td>[0]</td><td>[0]</td><td>[0]</td><td>[0]</td></tr>
                <tr><td></td><td>  </td><td>[1]</td><td>[1]</td><td>[1]</td><td>[1]</td><td>[1]</td><td>[1]</td><td>[1]</td><td>[1]</td><td>[1]</td><td>[1]</td></tr>
                <tr><td></td><td>  </td><td>[2]</td><td>[2]</td><td>[2]</td><td>[2]</td><td>[2]</td><td>[2]</td><td>[2]</td><td>[2]</td><td>[2]</td><td>[2]</td></tr>
                <tr><td></td><td>  </td><td>[3]</td><td>[3]</td><td>[3]</td><td>[3]</td><td>[3]</td><td>[3]</td><td>[3]</td><td>[3]</td><td>[3]</td><td>[3]</td></tr>
                <tr><td></td><td>  </td><td>[4]</td><td>[4]</td><td>[4]</td><td>[4]</td><td>[4]</td><td>[4]</td><td>[4]</td><td>[4]</td><td>[4]</td><td>[4]</td></tr>
                <tr><td></td><td>  </td><td>[5]</td><td>[5]</td><td>[5]</td><td>[5]</td><td>[5]</td><td>[5]</td><td>[5]</td><td>[5]</td><td>[5]</td><td>[5]</td></tr>

            </table>
        </div> </div>
</a>


    </div>
    <div v-else-if="item.questions.length===1" class="question" id="question" :ref="eachrefs">
        <div class="title"><strong><a>第{{item.title}}大题：</a></strong></div>
        <br>
        <template v-for="(question,qindex) in item.questions">
            <div class="tiny"> {{question.id}}、</div>
        </template>
    </div>
    <div v-else-if="item.questions[0].tinytype=='select'" class="question" id="question" :ref="eachrefs">
        <div class="titleselect"><strong><a>第{{item.title}}大题：</a></strong></div>
        <br>
        <template v-for="(question,qindex) in item.questions">
            <div class="tinyselect"> {{question.id}}、[A] [B] [C] [D]</div>
        </template>
    </div>
    <div v-else class="question" id="question" :ref="eachrefs">
        <div class="title"><strong><a>第{{item.title}}大题：</a></strong></div>
        <br>
        <template v-for="(question,qindex) in item.questions">
            <div class="tiny"> {{question.id}}、_______</div>
        </template>
    </div>
</template>

<script>

    export default {

        name: "questions.vue",
        props: ["item", "index"],
        mounted() {

            this.getXY(this.index.outer, this.index.inner);
            this.sendMsgToParent()

        },
        data() {
            return {
                titem: this.item,

                xx: '',
                yy: '',
                eachrefs:'element'+this.index.outer+this.index.inner,

            }
        },
        methods: {
            getXY: function (index, tindex) {
                let sposition;

                if (document.getElementById("bigbackground")) {
                    sposition = document.getElementById("bigbackground").scrollTop||0;
                }else{
                    console.log("不存在bigbackground");
                }
                //console.log("getXY:"+sposition);

                // var height = this.$refs[`${this.eachrefs}`].getBoundingClientRect().height;
                // var width = this.$refs.element.getBoundingClientRect().width;
                // var left = this.$refs.element.getBoundingClientRect().left;
                // var top = this.$refs.element.getBoundingClientRect().top;

                var height = this.$refs[`${this.eachrefs}`].getBoundingClientRect().height;
                var width = this.$refs[`${this.eachrefs}`].getBoundingClientRect().width;
                var left = this.$refs[`${this.eachrefs}`].getBoundingClientRect().left;
                var top =sposition+ this.$refs[`${this.eachrefs}`].getBoundingClientRect().top;

                this.titem.x1 = left;
                this.titem.y1 = top;
                this.titem.x2 = width + left;
                this.titem.y2 = height + top;

                this.titem.xx = width;
                this.titem.yy = height;

                this.xx = width;
                this.yy = height;

                   console.log(`${this.eachrefs}`+'高：'+height+"宽"+width+"左"+left+"上"+top)
                //   console.log("坐标："+this.item.x1+"------"+this.item.y1);
                //   console.log("坐标2："+this.item.x2+"------"+this.item.y2);

            },
            sendMsgToParent: function () {

                this.$emit("listenData", {



                    x1: parseFloat(this.titem.x1).toFixed(4),
                    y1: parseFloat(this.titem.y1).toFixed(4),
                    x2: parseFloat(this.titem.x2).toFixed(4),
                    y2: parseFloat(this.titem.y2).toFixed(4),
                    xx: parseFloat(this.titem.xx).toFixed(4),
                    yy: parseFloat(this.titem.yy).toFixed(4),
                    // x1: Math.ceil(this.titem.x1),
                    // y1: Math.ceil(this.titem.y1),
                    // x2: Math.ceil(this.titem.x2),
                    // y2: Math.ceil(this.titem.y2),
                    // xx: Math.ceil(this.titem.xx),
                    // yy: Math.ceil(this.titem.yy),

                })
            }
        },
    }
</script>

<style scoped>
    .question {
        width: 100%;
        min-height: 150px;

        top: 5%;
        left: 5%;
        /*position: absolute;*/
        overflow: hidden;
        text-align: center;
        border: 1px solid #000;
    }

    .stunum {
        width: 280px;
        min-height: 110px;
        float: right;
        margin-left: 5px;
        margin-right: 5px;
        margin-bottom: 5px;
        border: 1px solid #000;
        position:relative;
    }

    .stunumheader.td{
        border: 1px solid #000;
    }

  table{
      border: 0px solid #000;
      margin: 0;
  }

  td{
      margin: 0;

  }

    .infodiv {
        width: 200px;
        min-height: 110px;
        float: left;

        position:relative;
    }
    .tiny {
        float: left;
        margin: 5px;
        padding: 5px;
    }

    .title {
        float: left;
        margin: 5px;
        padding: 5px;
    }

    .titleselect {
        float: left;
        margin: 0px;
        padding: 0px;
    }
    .tinyselect {
        float: left;
        margin: 2px;
        padding: 2px;
    }

    .pointball {
        width: 5px;
        height: 5px;
        background-color: red;
        position: absolute;
        border-radius: 50%;
        -moz-border-radius: 50%;
        -webkit-border-radius: 50%;
    }
</style>