<template>
  <div class="formal-paper-con">
    <div class="test-title">
      <h1 class="test-name">
        <span @click="editText('title')" v-show="!isTitleInput">{{title}}</span>
        <input type="text" class="name-input" ref="input" placeholder="title" v-model="title" v-show="isTitleInput" @blur="finishEdit('title')" @keyup.enter="finishEdit('title')">
      </h1>
      <h2>
        <span @click="editText('subTitle')" v-show="!isSubtitleInput">{{subtitle}}</span>
        <input type="text" class="sub-title-input"  placeholder="subtitle" v-model="subtitle" v-show="isSubtitleInput" autofocus @blur="finishEdit('subTitle')" @keyup.enter="finishEdit('subTitle')">
      </h2>
    </div>

    <div class="test-time">
      <div class="time">考试时间：
        <span @click="editText('time')" v-show="!isTimeInput">{{time}}</span>
        <input type="text" class="time-input" ref="input" placeholder="time" v-model="time" v-show="isTimeInput"  @blur="finishEdit('time')" @keyup.enter="finishEdit('time')">
      </div>
      <div class="score">满分：
        <span @click="editText('score')" v-show="!isScoreInput">{{score}}</span>
        <input type="text" class="score-input" ref="input" placeholder="score" v-model="score" v-show="isScoreInput" autofocus @blur="finishEdit('score')" @keyup.enter="finishEdit('score')">
      </div>
    </div>

    <ul class="student-info">
      <li class="item">姓名：____________</li>
      <li class="item">班级：____________</li>
      <li class="item">学号：____________</li>
    </ul>

    <div class="mark-table">
      <table class="paper-score">
        <tbody>
        <tr>
          <th>题号</th>
          <td>一</td>
          <td>二</td>
          <td>三</td>

        </tr>
        <tr>
          <th>评分</th>
          <td>&nbsp;</td>
          <td>&nbsp;</td>
          <td>&nbsp;</td>
        </tr>
        </tbody>
      </table>
    </div>

    <div class="paper-rule">
      <p class="text">* 注意事项：</p>
      <div class="rule-content">
        1、填写答题卡的内容用2B铅笔填写<br>
        2、提前 xx 分钟收取答题卡
      </div>
    </div>

    <div class="formal-paper-body">
      <div class="paper-subject-list" v-for="(subject,s) in formalPaperData.questionPatterns">
        <div class="item-part-paper">
          <h3>第 I 卷  客观题</h3>
          <span class="notes">第I卷的注释</span>
        </div>
        <div class="title-details">
          <div class="paper-type" @mouseenter="hoverEvent(s)" @mouseleave="hoverEvent(s)">
            <table class="type-table">
              <tbody>
              <tr>
                <th>阅卷人</th>
                <td></td>
              </tr>
              <tr>
                <th>得&nbsp;&nbsp;分</th>
                <td></td>
              </tr>
              </tbody>
            </table>
            <div class="type-title">一、{{subject.name}} (共{{subject.len}}题; 共{{subject.totalScore}}分)</div>
            <div class="hover-dialog" v-show="subject.isShow">
              批量设置分数
            </div>
          </div>
        </div>
        <!--试题详情-->
        <div class="test-questions-detail">
          <div class="item-test" v-for="(question,q) in subject.questions" @mouseenter="subjectHoverEvent(s,q)" @mouseleave="subjectHoverEvent(s,q)">
            <div class="questions">
              {{q+1}}. &nbsp;
              <span class="questions-score" >({{question.score}}分)</span>
              <span class="questions-detail">
                {{question.question}}
                <span v-if="subject.name == '单选题'">( &nbsp;&nbsp; )</span>
              </span>
              <!--<div class="questions-image">-->
                <!--<img src="http://tikupic.21cnjy.com/ac/44/ac442d59dfa7b82f034ca7f126150420.png" alt="">-->
              <!--</div>-->
            </div>
            <!--选择题 选项列表-->
            <div class="select-answers-area" v-if="subject.name == '单选题'">
              <span class="item-answers" v-for="(order,i) in ['A','B','C','D']">
                <span class="answers-order">{{order}}、</span>
                <span class="answers-des">
                  {{question.choices[i]}}
                  <!--<img src="http://tikupic.21cnjy.com/a9/5c/a95c8186a7f7082bbcc27b638b32097c.png" style="width: 99px;height: 102px;" alt="">-->
                </span>
              </span>
            </div>

            <!--除选择题之外的 子题列表-->
            <div class="exam-child-list" v-else>
              <div class="questions">
                <span>(1)</span>
                 先简化，再求值
              </div>
            </div>
            <div class="subject-hover" v-show="question.isSetting">
              <span class="hover-item">查看解析</span>
              <span class="hover-item">设定分数</span>
              <span class="hover-item">上移</span>
              <span class="hover-item">下移</span>
              <span class="hover-item">换题</span>
              <span class="hover-item">删除</span>
            </div>
          </div>



        </div>


      </div>
    </div>
  </div>
</template>

<script>
  import Vue from 'vue';
  export default {
    name: "formal-paper",
    data() {
      return {
        isShow:false,
        isTitleInput:false,
        isSubtitleInput:false,
        isTimeInput:false,
        isScoreInput:false,
        title:'2019年1月10号数学试卷',
        subtitle:'数学试卷',
        time:'*  * 分钟',
        score:'*  * 分',
        formalPaperData:{
          title:'2019年1月10号数学试卷',
          subtitle:'数学试卷',
          questionPatterns:[
            {
              id:1,
              name:'单选题',
              questions:[
                {
                  id:11,
                  score:2,
                  question:'不一定在三角形内部的线段是',
                  choices:['三角形的角平分线','三角形的中线','三角形的高','三角形的中位线']
                },
                {
                  id:12,
                  score:2,
                  question:'不一定在三角形内部的线段是',
                  choices:['三角形的角平分线','三角形的高','三角形的中线','三角形的中位线']
                }
              ]
            },
            {
              id:2,
              name:'填空题',
              questions:[
                {
                  id:21,
                  score:2,
                  question:'已知△ABC∽△DEF，且BC=5cm，EF=3cm，若S△ABC=25cm,则S=______。',
                  choices:['三角形的角平分线','三角形的中线','三角形的高','三角形的中位线']
                },
                {
                  id:22,
                  score:2,
                  question:'已知△ABC∽△DEF，且BC=5cm，EF=3cm，若S△ABC=25cm,则S=______。',
                  choices:['三角形的角平分线','三角形的高','三角形的中线','三角形的中位线']
                }
              ]
            },
            {
              id:2,
              name:'计算题',
              questions:[
                {
                  id:21,
                  score:2,
                  question:'已知△ABC∽△DEF，且BC=5cm，EF=3cm，若S△ABC=25cm,则S=______。',
                  choices:['三角形的角平分线','三角形的中线','三角形的高','三角形的中位线']
                },
                {
                  id:22,
                  score:2,
                  question:'已知△ABC∽△DEF，且BC=5cm，EF=3cm，若S△ABC=25cm,则S=______。',
                  choices:['三角形的角平分线','三角形的高','三角形的中线','三角形的中位线']
                }
              ]
            }
          ]
        }

      }
    },
    created (){
       this.formalPaperData.questionPatterns.forEach(function (item,index) {
         var len = item.questions.length;
         item.len = len;
         item.totalScore = item.questions[0].score * len;
         item.isShow = false;
         item.questions.forEach(function (q,k) {
           q.isSetting =false;
         })
       });
    },
    methods:{
      hoverEvent(s){
        this.formalPaperData.questionPatterns[s].isShow = !this.formalPaperData.questionPatterns[s].isShow;
        Vue.set(this.formalPaperData.questionPatterns,s,this.formalPaperData.questionPatterns[s])
      },
      subjectHoverEvent(s,q){
        this.formalPaperData.questionPatterns[s].questions[q].isSetting = !this.formalPaperData.questionPatterns[s].questions[q].isSetting;
        Vue.set(this.formalPaperData.questionPatterns[s].questions,q,this.formalPaperData.questionPatterns[s].questions[q])
      },
      editText(sign){

        switch (sign){
          case 'title':
            this.isTitleInput = true;
            // console.log(this.$refs['input'],"this.$refs['input']");
            // this.$refs['input'].focus();
            break;
          case 'subTitle':
            this.isSubtitleInput = true;
            break;
          case 'time':
            this.isTimeInput = true;
            break;
          case 'score':
            this.isScoreInput = true;
            break;
        }
      },
    //结束编辑
      finishEdit(sign){
        switch (sign){
          case 'title':
            this.isTitleInput = false;
            break;
          case 'subTitle':
            this.isSubtitleInput = false;
            break;
          case 'time':
            this.isTimeInput = false;
            break;
          case 'score':
            this.isScoreInput = false;
            break;
        }
      }
    }
  }
</script>

<style scoped>
  .formal-paper-con {
    width: 65%;
    border-left: 1px dashed #e6e6e6;
    padding: 10px;
    margin-top: 20px;
    margin-left: 33%;
    font-size: 14px;
    color: #333;
  }
.test-title span{
  display: inline;
  cursor: pointer;
}
.test-time span{
  cursor: pointer;
}
  .test-title .test-name>span:hover,.test-title h2>span:hover,.test-name .name-input,.test-title .sub-title-input,.test-time span:hover,.test-time input{
    border: 1px solid #f1b48f;
    background: rgb(240, 225, 216);
  }
  .test-title .test-name {
    height: 50px;
    line-height: 50px;
    font-size: 18px;
    text-align: center;
  }
  .test-name .name-input{
    width: 600px;
  }
  .test-title .sub-title-input{
    width: 200px;
  }

  .test-name .name-input,.test-title .sub-title-input,.test-time input{
    height: 25px;
    margin-top: 12px;
    padding: 5px;
    font-size: 18px;
    color: #333;
    font-weight: 700;
    -webkit-border-radius: 4px;
    -moz-border-radius: 4px;
    border-radius: 4px;
  }
  .test-time>div input{
    width: 100px;
    height: 18px;
    line-height: 18px;
  }
  .test-title h2 {
    text-align: center;
    font-size: 16px;
  }

  .test-time, .student-info, .formal-paper-body {
    margin-top: 20px;
  }

  .test-time > div {
    display: inline-block;
  }

  .test-time .time {
    margin-right: 24px;
  }

  .student-info .item {
    display: inline-block;
    margin-right: 10px;
  }
  .mark-table{
    padding: 0 60px;
  }
  .mark-table .paper-score{
    margin: 20px auto;
    width: 100%;
    font-size: 14px;
    border: 1px solid #dcdcdc;
    border-bottom: none;
  }
  .mark-table .paper-score tr{
    height: 40px;
    line-height: 40px;
  }
  .paper-score tr th{
    font-weight: 400;
    text-align: center;
    border-bottom: 1px solid #dcdcdc;
  }
  .paper-score tr td{
    text-align: center;
    border-left: 1px solid #dcdcdc;
    border-bottom: 1px solid #dcdcdc;
  }
  .paper-rule .rule-content{
    margin-top: 10px;
    line-height: 30px;
  }
  .formal-paper-body .item-part-paper,.paper-type,.item-test{
    position: relative;
  }
  .item-part-paper h3{
    font-size: 16px;
    line-height: 18px;
    font-weight: 700;
    text-align: center;
  }
  .item-part-paper .notes{
    position: absolute;
    bottom: 0;
    left: 0;
    font-size: 12px;
    color: #999;
  }
  .paper-type{
    margin: 15px 0;
    height: 80px;
    background: #eee;
    border: 1px solid transparent;
  }
  .paper-type .hover-dialog{
    width: 122px;
    height: 36px;
    border: 1px solid #1890ff;
    border-bottom: 0;
  }
  .paper-type .hover-dialog,.subject-hover{
    position: absolute;
    z-index: 2;
    top: -38px;
    right: -1px;
    line-height: 36px;
    height: 36px;
    overflow: hidden;
    text-align: center;
    cursor: pointer;
    color: #1890FF;
    background: #fff;
  }
  .subject-hover{
    font-size: 0;
    border-top: 1px solid #1890ff;
  }
  .subject-hover .hover-item:first-child{
    border-left: 1px solid #1890ff;
  }
  .subject-hover .hover-item{
    font-size: 14px;
    padding: 11px 16px;
    border-right: 1px solid #1890ff;
  }
  .paper-type .hover-dialog:hover,.subject-hover .hover-item:hover{
    background: #1890FF;
    color: #fff;
  }
  .paper-type .type-table{
    margin: 10px;
    width: 100px;
    float: left;
    border: 1px solid #dcdcdc;
    border-bottom: none;
    background: #fff;
  }
  .type-table tr td,.type-table tr th{
    width: 50%;
    height: 30px;
    line-height: 30px;
    text-align: center;
    border-bottom: 1px solid #dcdcdc;
  }
  .type-table tr td{
    border-left: 1px solid #dcdcdc;
  }
  .type-title{
    float: left;
    font-size: 14px;
    font-weight: 700;
    line-height: 80px;
    margin-left: 20px;
  }
  .test-questions-detail .item-test{
    background: #fff;
    zoom: 1;
    line-height: 30px;
    margin: 14px 0;
    cursor: move;
    padding: 0 15px 10px;
    border: 1px solid transparent;
  }
  .test-questions-detail .item-test:hover,.paper-type:hover{
    border: 1px solid #1890FF;
  }
  .questions .questions-score{
    color: rgba(0,0,0,0.45);
    margin-right: 12px;
  }
 .questions .questions-detail{
   line-height: 22px;
 }
</style>
