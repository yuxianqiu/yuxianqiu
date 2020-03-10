<template>
  <div class="content">
      <el-row>
             <span>日期：</span>
             <el-date-picker
                     v-model="time"
                     type="date"
                     placeholder="选择日期">
             </el-date-picker>
              <div class="dw" style="margin-left: 500px;">
                  <div class="ys1 color1"></div>
                  <span class="text1">在线预约</span>
                  <div class="ys2 color2"></div>
                  <span class="text2 ">非在线预约</span>
              </div>

      </el-row>
     <el-row  style="margin-top: 20px;">
         <table >
             <tr>
                 <th class="titBg"><span>小时</span></th>
                 <th class="titBg"><span>已分配/总资源</span></th>
                 <th class="titBg">
                     <span >未分配</span>
                 </th>
<!--                 这里是动态表头  用方法  return返回-->
                 <th class="titBg"><span class="block">{{formData[0].SA_cdn}}</span> <span>({{res.la2}})</span></th>
                 <th class="titBg"><span class="block">{{formData[0].SA_mrx}}</span>  <span>({{res.la3}})</span></th>
                 <th class="titBg"><span class="block">{{formData[0].SA_oyyn}}</span>  <span>({{res.la4}})</span></th>
                 <th class="titBg"><span class="block">{{formData[0].SA_ww}}</span>  <span>({{res.la5}})</span></th>
                 <th class="titBg"><span class="block">{{formData[0].SA_zwh}}</span>  <span>({{res.la6}})</span></th>
             </tr>
             <tr v-for="(data,index) in formData" :key="index">
                 <th  class="addYs">{{data.hour}}</th>
                 <th   :class="{color3:data.fp1==data.fp2}" class="addYs">{{data.fp1}}/{{data.fp2}}</th>
                 <th @dragenter="dragenter($event,index)"
                     @dragover="dragover($event,index)"
                     @dragleave="dragleave($event,index)"
                     @drop="drop($event,index)"
                     :cs="'2'">
                     <div v-if="formData1[index].wpf.indexOf(',')!='-1'" :cs="'2'" :class="{b_fff:true,'b_hover':formData[index].fp1<formData[index].fp2&&pd1&&pd2}">
<!--                      原   <div  draggable="true" :cs="'2'" @dragstart="tableDragstart($event,index)" @drag="tableDrag($event,index)"  @dragend="tableDragend($event,index)" v-for="(item,index1) in formData1[index].wpf.split(',')" class="addYs" :key="index1" :class="{color1:item.split('_')[1]=='1','color2':item.split('_')[1]=='2'}">{{item.split('_')[0]}}</div>-->
                         <div :cs="'2'" style="display: flex;justify-content: center;"><div :cs="'2'" class="ellipse" @click="changeShow(formData1[index].wpf,'2',index)">{{formData1[index].wpf.split(',').length}}</div></div>
                     </div>
<!--                     <div v-else-if="formData1[index].wpf.indexOf('_')!='-1'">一个的时候</div>-->
                     <div v-else draggable="true" :cs="'2'" @dragstart="tableDragstart($event,index)" @drag="tableDrag($event,index)"  @dragend="tableDragend($event,index)" class="addYs"  :class="{color1:formData1[index].wpf.split('_')[1]=='1','color2':formData1[index].wpf.split('_')[1]=='2','b_fff':true,'b_hover':formData[index].fp1<formData[index].fp2&&formData1[index].wpf.split('_')[0]==''&&pd1&&pd2}">{{formData1[index].wpf.indexOf('_')!='-1'?formData1[index].wpf.split('_')[0] :''}}</div>
                 </th>
                 <th
                       :cs="'3'" @dragenter="dragenter($event,index)"
                      @dragover="dragover($event,index)"
                      @dragleave="dragleave($event,index)"
                      @drop="drop($event,index)" >
                     <div  @dragstart="tableDragstart($event,index)" @drag="tableDrag($event,index)"  @dragend="tableDragend($event,index)"
                           draggable="true" :cs="'3'" :class="{color4:formData1[index].SA_cdn.split('_')[1]=='0','color1':formData1[index].SA_cdn.split('_')[1]=='1','color2':formData1[index].SA_cdn.split('_')[1]=='2','b_fff':true,'b_hover':formData[index].fp1<formData[index].fp2&&formData1[index].SA_cdn==''&&pd1}" class="addYs">
                         {{formData1[index].SA_cdn.split('_')[1]=='0'?'请假':formData1[index].SA_cdn.split('_')[0]}}
                     </div>
                 </th>
                 <th
                     :cs="'4'" @dragenter="dragenter($event,index)"
                     @dragover="dragover($event,index)"
                     @dragleave="dragleave($event,index)"
                     @drop="drop($event,index)" >
                     <div draggable="true" :cs="'4'" @dragstart="tableDragstart($event,index)" @drag="tableDrag($event,index)"  @dragend="tableDragend($event,index)"
                          :class="{color4:formData1[index].SA_mrx.split('_')[1]=='0','color1':formData1[index].SA_mrx.split('_')[1]=='1','color2':formData1[index].SA_mrx.split('_')[1]=='2','b_fff':true,'b_hover':formData[index].fp1<formData[index].fp2&&formData1[index].SA_mrx==''&&pd1}" class="addYs">
                         {{formData1[index].SA_mrx.split('_')[1]=='0'?'请假':formData1[index].SA_mrx.split('_')[0]}}
                     </div>
                 </th>
                 <th
                     :cs="'5'" @dragenter="dragenter($event,index)"
                     @dragover="dragover($event,index)"
                     @dragleave="dragleave($event,index)"
                     @drop="drop($event,index)" >
                    <div  @dragstart="tableDragstart($event,index)" @drag="tableDrag($event,index)"  @dragend="tableDragend($event,index)"
                           draggable="true" :cs="'5'" :class="{color4:formData1[index].SA_oyyn.split('_')[1]=='0','color1':formData1[index].SA_oyyn.split('_')[1]=='1','color2':formData1[index].SA_oyyn.split('_')[1]=='2','b_fff':true,'b_hover':formData[index].fp1<formData[index].fp2&&formData1[index].SA_oyyn==''&&pd1}" class="addYs">
                        {{formData1[index].SA_oyyn.split('_')[1]=='0'?'请假':formData1[index].SA_oyyn.split('_')[0]}}
                    </div>
                 </th>
                 <th :cs="'6'" @dragenter="dragenter($event,index)"
                     @dragover="dragover($event,index)"
                     @dragleave="dragleave($event,index)"
                     @drop="drop($event,index)" >
                     <div  draggable="true" :cs="'6'"
                           :class="{color4:formData1[index].SA_ww.split('_')[1]=='0','color1':formData1[index].SA_ww.split('_')[1]=='1','color2':formData1[index].SA_ww.split('_')[1]=='2','b_hover':formData[index].fp1<formData[index].fp2&&formData1[index].SA_ww.split('_')[0]==''&&pd1,'b_fff':true}" class="addYs"
                           @dragstart="tableDragstart($event,index)" @drag="tableDrag($event,index)"  @dragend="tableDragend($event,index)"
                     >
                         {{formData1[index].SA_ww.split('_')[1]=='0'?'请假':formData1[index].SA_ww.split('_')[0]}}
                     </div>
                 </th>
                 <th @dragenter="dragenter($event,index)"
                     @dragover="dragover($event,index)"
                     @dragleave="dragleave($event,index)"
                     @drop="drop($event,index)" :cs="'7'"  >
                     <div draggable="true"
                          @dragstart="tableDragstart($event,index)" @drag="tableDrag($event,index)"  @dragend="tableDragend($event,index)"
                          :cs="'7'"
                          :class="{color4:formData1[index].SA_zwh.split('_')[1]=='0','color1':formData1[index].SA_zwh.split('_')[1]=='1','color2':formData1[index].SA_zwh.split('_')[1]=='2','b_hover':formData[index].fp1<formData[index].fp2&&formData1[index].SA_zwh.split('_')[0]==''&&pd1,'b_fff':true}" class="addYs"
                     >
                         {{formData1[index].SA_zwh.split('_')[1]=='0'?'请假':formData1[index].SA_zwh.split('_')[0]}}
                     </div>
                 </th>
             </tr>
         </table>
     </el-row>
      <el-row v-if="show"  class="ysShow" style="margin-top: 20px;" >
          <i class="el-icon-circle-close ysShow_close" @click="closeShow"></i>
          <div v-for="(item,index) in tkData" :key="index"  @dragstart="dragstart($event,index)" @drag="drag($event,index)"  @dragend="dragend($event,index)" class="bg" :class="{ color1: item.name.split('_')[1] == '1', 'color2': item.name.split('_')[1] == '2' }" draggable="true"  >{{item.name.split('_')[0]}}</div>
      </el-row>
<!--      <Fuzhi />-->
<!--      <div >{{this.ceshi('cyx')}}</div>-->
<!--      <div style="width: 100px;height: 100px;" class="b_hover b_fff">1</div>-->
      <el-dialog
              title="提示"
              :visible.sync="dialogVisible"
              width="30%"
              :before-close="handleClose">
          <span>确定把它加入表格？</span>
          <span slot="footer" class="dialog-footer">
    <el-button @click="dialogVisible = false">取 消</el-button>
    <el-button type="primary" @click="changeTable">确 定</el-button>
  </span>
      </el-dialog>
      <el-dialog
              title="提示"
              :visible.sync="tableDialogVisible"
              width="30%"
              :before-close="handleCloseTwo">
          <span>确定把它加入表格？</span>
          <span slot="footer" class="dialog-footer">
    <el-button @click="tableDialogVisible = false">取 消</el-button>
    <el-button type="primary" @click="changeTableTwo">确 定</el-button>
  </span>
      </el-dialog>

  </div>
</template>

<script>
  // import Fuzhi from  './fuzhi'
    export default {
        name: 'HelloWorld',
        data() {
            return {
                pd1:false,   // 原 false
                pd2:true,    // 原 true
                textContent:'', // 第三列多个拖动的时候  拖动文字方块
                table_x:'', // 拖动开始 x
                table_y:'',  // 拖动开始 y
                num_key:'',
                index_x:'',   // 放置坐标x
                index_y:'',   // 放置坐标y
                index:null,
                tkData:[
                //     {
                //     name:'京B56369_1',
                //     index_x:'2',
                //     index_y:'1'
                // },{
                //    name:'闽A123456_2',
                //     index_x:'2',
                //     index_y:'1'
                // },{
                //      name:'京B123456_2',
                //     index_x:'2',
                //     index_y:'14'
                //  }
                ],
                show:false,
                time:'',
                dialogVisible: false,
                tableDialogVisible:false,
                res:{
                    la1:'0',
                    la2:'0',
                    la3:'1',
                    la4:'0',
                    la5:'0',
                    la6:'0',
                },
                formData:[{hour:'9:30',fp1:'0',fp2:'2',SA_cdn:'SA成大年',SA_mrx:'SA慕容雪',SA_oyyn:'SA欧阳延年',SA_ww:'SA王文',SA_zwh:'SA周文化'},
                    {hour:'9:30',fp1:'3',fp2:'3',SA_cdn:'',},
                    {hour:'10:00',fp1:'0',fp2:'2',SA_cdn:''},
                    {hour:'10:30',fp1:'0',fp2:'2',SA_cdn:''},
                    {hour:'11:00',fp1:'0',fp2:'2',SA_cdn:''},
                    {hour:'11:30',fp1:'0',fp2:'2',SA_cdn:''},
                    {hour:'12:00',fp1:'0',fp2:'2',SA_cdn:''},
                    {hour:'12:30',fp1:'0',fp2:'2',SA_cdn:''},
                    {hour:'13:00',fp1:'0',fp2:'2',SA_cdn:''},
                    {hour:'13:30',fp1:'0',fp2:'2',SA_cdn:''},
                    {hour:'14:00',fp1:'0',fp2:'2',SA_cdn:''},
                    {hour:'14:30',fp1:'0',fp2:'2',SA_cdn:''},
                    {hour:'15:00',fp1:'0',fp2:'2',SA_cdn:''},
                    {hour:'15:30',fp1:'0',fp2:'2',SA_cdn:''},
                    {hour:'16:00',fp1:'1',fp2:'2',SA_cdn:''},
                    {hour:'16:30',fp1:'0',fp2:'2',SA_cdn:''},
                    {hour:'17:00',fp1:'1',fp2:'2',SA_cdn:''},
                    {hour:'17:30',fp1:'0',fp2:'2',SA_cdn:''},
                ],
                formData1:[    //过滤之后的数据
                {wpf:'',SA_cdn:'',SA_mrx:'',SA_oyyn:'',SA_ww:'',SA_zwh:''},
                {wpf:'闽A123456_2,京B56369_1,粤A123456_2',SA_cdn:'',SA_mrx:'',SA_oyyn:'',SA_ww:'',SA_zwh:''},
                    {wpf:'',SA_cdn:'',SA_mrx:'',SA_oyyn:'',SA_ww:'',SA_zwh:''},
                    {wpf:'',SA_cdn:'',SA_mrx:'',SA_oyyn:'',SA_ww:'',SA_zwh:''},
                    {wpf:'',SA_cdn:'',SA_mrx:'',SA_oyyn:'',SA_ww:'',SA_zwh:''},
                    {wpf:'',SA_cdn:'',SA_mrx:'xx_0',SA_oyyn:'',SA_ww:'',SA_zwh:''},
                    {wpf:'',SA_cdn:'',SA_mrx:'xx_0',SA_oyyn:'',SA_ww:'',SA_zwh:''},
                    {wpf:'',SA_cdn:'',SA_mrx:'xx_0',SA_oyyn:'',SA_ww:'',SA_zwh:''},
                    {wpf:'',SA_cdn:'',SA_mrx:'xx_0',SA_oyyn:'',SA_ww:'',SA_zwh:''},
                    {wpf:'',SA_cdn:'',SA_mrx:'',SA_oyyn:'',SA_ww:'',SA_zwh:''},
                    {wpf:'',SA_cdn:'',SA_mrx:'',SA_oyyn:'',SA_ww:'',SA_zwh:''},
                    {wpf:'',SA_cdn:'',SA_mrx:'',SA_oyyn:'',SA_ww:'',SA_zwh:''},
                    {wpf:'',SA_cdn:'',SA_mrx:'',SA_oyyn:'',SA_ww:'',SA_zwh:''},
                    {wpf:'',SA_cdn:'',SA_mrx:'',SA_oyyn:'',SA_ww:'',SA_zwh:''},
                    {wpf:'京B123456_2',SA_cdn:'',SA_mrx:'',SA_oyyn:'',SA_ww:'',SA_zwh:''},
                    {wpf:'',SA_cdn:'',SA_mrx:'',SA_oyyn:'',SA_ww:'',SA_zwh:''},
                    {wpf:'',SA_cdn:'',SA_mrx:'闽A654321_1',SA_oyyn:'',SA_ww:'',SA_zwh:''},
                    {wpf:'',SA_cdn:'',SA_mrx:'',SA_oyyn:'',SA_ww:'',SA_zwh:''},
            ],

            };
        },
        component:{
          // Fuzhi:Fuzhi,
        },
        mounted() {

        },
        methods: {
            // ceshi (x){
            //     if (x == 'cyx'){
            //         return '测试成功'
            //     }
            // },
            handleClose() {
                this.$confirm('确认关闭？')
                    .then(
                        this.dialogVisible = false
                    )
                    .catch(
                        this.dialogVisible = false
                    );
            },
            handleCloseTwo() {
                this.$confirm('确认关闭？')
                    .then(
                        this.tableDialogVisible = false
                    )
                    .catch(
                        this.tableDialogVisible = false
                    );
            },
            changeShow(data,index_x,index_y){
                this.show = true
                this.tkData = []
                data.split(',').forEach(x =>{
                    let b = {
                        name:x,
                        index_x:index_x,
                        index_y:index_y
                    }
                    this.tkData.push(b)
                })
                console.log(this.tkData,'xxx')
            },
            closeShow(){
                this.show = false
            },
            // 拖动事件
            dragstart(event,index) {
                console.log('开始拖动',index)
                this.index = index
                this.pd1 = true
                // 这里可以知道x,y   拖动第二行的  也就知道哪些地方可以落  哪些不能落

            },
            dragend (event,index){
                console.log(event,index,'拖动结束');
                this.pd1 = false
            },
            drag(event,index) {  //拖动中
                console.log('拖动中',index);
            },
            // 放下事件

            drop(e,index){

                console.log('------------------------- 执行了！！！！！！ -------------------------------',e,index)
               console.log('handle_drop-当元素在目的地放下时触发',e,e.srcElement.attributes.cs.value,index);
               // console.log(Object.keys(this.formData1[index])[e.srcElement.cellIndex-2],'获取对应的key值')
                console.log(Object.keys(this.formData1[index])[e.srcElement.attributes.cs.value],'获取对应的key值')
                // console.log(e.srcElement.attributes.cs.value,'3')
                this.index_x = e.srcElement.attributes.cs.value
                this.index_y = index
                console.log(this.index_x,this.index_y,'放置的坐标')
                if (this.index != null){ // 非表格内拖动
                    console.log(this.index,'拖动的方块-------底下')
                    // this.dialogVisible = true
                    this.changeTable()
                } else {     // 表格内拖动
                    console.log(this.index,'拖动的方块-------底下')
                    // this.tableDialogVisible = true
                    this.changeTableTwo()
                }
                e.preventDefault()
            },
            dragenter(e,index){
                console.log('handle_enter-当元素进入目的地时触发',e,index);
                e.preventDefault()
            },
            dragover(e,index){
                console.log('handle_over-当元素在目的地时触发',e,index);
                e.preventDefault()
            },
            dragleave(e,index){
                console.log('handle_leave-当元素离开目的地时触发',e,index);
                e.preventDefault()
            },
            changeTable () {
                const key = Object.keys(this.res)[Number(this.index_x)-2]  //数量对应的key值
                console.log(this.res[Object.keys(this.res)[Number(this.index_x)-2]],'对应的数量')
                // formData1  展示的数据
                // tkData  地下弹框的数据
                // res 表头数量  formData头数据和一二栏数据
                //  this.tkData[this.index].index_x    Number(this.tkData[this.index].index_x)-2 ((数据所在行))  this.tkData[this.index].index_y    自身所在与列
                //  this.index_x  this.index_y  移动到的行列  Number(this.index_x）-2
                // 填入表格
                const key1 = Object.keys(this.formData1[Number(this.index_y)])[Number(this.index_x)-2] // 要填入表格的key值
                const key2 = Object.keys(this.formData1[Number(this.tkData[this.index].index_y)])[Number(this.tkData[this.index].index_x)-2]
                console.log(key1,key2,'key值')
                if (this.formData[this.index_y].fp1 == this.formData[this.index_y].fp2) {  // 满了
                    this.$notify({
                        title: '警告',
                        message: '该位置不能放置',
                        type: 'warning'
                    });
                    return;
                }
                if (this.tkData[this.index].index_x==this.index_x&&this.tkData[this.index].index_y==this.index_y) {
                    this.$notify({
                        title: '警告',
                        message: '不能把自己放到自己里',
                        type: 'warning'
                    });
                    return;
                }
                this.res[key]++
                if(this.index_x == '2') { //在一行的情况
                      // ----------- 这里要加一个判断(一个或者为空)（ 第三列如果满（pf1==pf2)的时候 （下面也要价格如果已经有车牌号时  不添加） --------
                    if (this.formData1[this.index_y][key1].indexOf(',')== '-1'){
                        if (this.formData1[this.index_y][key1].indexOf('_')=='-1'){
                            this.formData1[this.index_y][key1] = this.tkData[this.index].name
                        } else {
                            let d1 = []
                            d1.push(this.formData1[this.index_y][key1])
                            d1.push(this.tkData[this.index].name)
                            this.formData1[this.index_y][key1] = d1.join(',')
                        }

                    } else {
                        let a  =this.formData1[this.index_y][key1].split(',')
                        let b = []
                        a.forEach(x =>{
                            b.push(x)
                        })
                        b.push(this.tkData[this.index].name)
                        this.formData1[this.index_y][key1] = b.join(',')
                    }
                    this.formData[this.index_y].fp1++ //移动到的那行数量加一
                    this.formData[this.tkData[this.index].index_y].fp1 -- // 移动之后自身所在行数量减1
                  if (this.formData1[this.tkData[this.index].index_y][key2].indexOf(',') == '-1') { //  如果 第三列  数量就一个的时候
                      this.formData1[this.tkData[this.index].index_y][key2] = ''
                  } else{  // 如果三列  多个的时候
                      let a = this.formData1[this.tkData[this.index].index_y][key2].split(',')
                      let b = []
                      a.forEach(x=>{
                          if (x != this.tkData[this.index].name){
                              b.push(x)
                          }
                      })
                      if (b.length == 1){
                          this.formData1[this.tkData[this.index].index_y][key2] = b[0]
                      } else {
                          this.formData1[this.tkData[this.index].index_y][key2] = b.join(',')
                      }

                  }
                   //  最后改变它的行和列的信息
                    this.tkData[this.index].index_x = this.index_x
                    this.tkData[this.index].index_y = this.index_y
                } else {  // 不在三行的情况
                    //  不在第三列的时候的逻辑
                    this.formData1[this.index_y][key1] = this.tkData[this.index].name
                    // ------------ 以下与上面逻辑重合  ----------  //
                    this.formData[this.index_y].fp1++ //移动到的那行数量加一
                    this.formData[this.tkData[this.index].index_y].fp1 -- // 移动之后自身所在行数量减1
                    if (this.formData1[this.tkData[this.index].index_y][key2].indexOf(',') == '-1') { //  如果 第三列  数量就一个的时候
                        this.formData1[this.tkData[this.index].index_y][key2] = ''
                    } else{  // 如果三列  多个的时候
                        let a = this.formData1[this.tkData[this.index].index_y][key2].split(',')
                        let b = []
                        a.forEach(x=>{
                            if (x != this.tkData[this.index].name){
                                b.push(x)
                            }
                        })
                        if (b.length == 1){
                            this.formData1[this.tkData[this.index].index_y][key2] = b[0]
                        } else {
                            this.formData1[this.tkData[this.index].index_y][key2] = b.join(',')
                        }

                    }

                }

                //  这里要判断下  是否可以添加。。。 或者是否本身
                this.tkData.splice(this.index,1)
                this.index = null
               if (this.tkData.length<=1){
                   this.show = false
               }
                this.dialogVisible = false
            },
            changeTableTwo () { // 表格内拖动方法
                this.tableDialogVisible = false
                // -------              方法开始  -------------//


                // formData1  展示的数据
                // res 表头数量  formData头数据和一二栏数据
                //  this.tkData[this.index].index_x    Number(this.tkData[this.index].index_x)-2 ((数据所在行))  this.tkData[this.index].index_y    自身所在与列
                //  this.index_x  this.index_y  移动到的行列  Number(this.index_x）-2  移动的列index
                //   this.table_x  this.table_y 开始行列
                const key = Object.keys(this.res)[Number(this.table_x)-2]  // 开始  res数量对应的key值
                const key_end = Object.keys(this.res)[Number(this.index_x)-2]  // 结束  res数量对应的key值
                console.log(key,key_end,'xxxxx')
                console.log(this.res[Object.keys(this.res)[Number(this.table_x)-2]],'对应的数量')
                console.log(Object.keys(this.formData1[this.table_y])[Number(this.table_x)-2],'ceshi')
                let key2 = Object.keys(this.formData1[this.table_y])[Number(this.table_x)-2]   // 拉起的数据key
                let key1 = Object.keys(this.formData1[this.index_y])[Number(this.index_x)-2]   // 落下的数据key
                console.log(this.formData1[this.table_y][key2],'this.formData1[this.table_y][key2]')
                console.log(this.formData1[this.table_y][key1],'this.formData1[this.table_y][key1]')
                if (this.formData[this.index_y].fp1 == this.formData[this.index_y].fp2) {  // 满了
                    this.$notify({
                        title: '警告',
                        message: '该位置不能放置',
                        type: 'warning'
                    });
                    return;
                }
                if (this.index_x==this.table_x&&this.index_y==this.table_y) {
                    this.$notify({
                        title: '警告',
                        message: '不能把自己放到自己里',
                        type: 'warning'
                    });
                    return;
                }


                if(this.formData1[this.table_y][key2].indexOf('_0')!='-1') { //  当拖动的是请假时
                    if (this.index_x=='2'){   //  请假拖到为分配区时
                        this.$notify({
                            title: '警告',
                            message: '请假不能拖动到未分配区',
                            type: 'warning'
                        });
                        return
                    } else {   //  拖到非分配区时
                        console.log('拖到非分配区时')
                        if (this.formData1[this.index_y][key1].indexOf('_')!='-1'){
                            this.$notify({
                                title: '警告',
                                message: '该位置已存在不能放置',
                                type: 'warning'
                            });
                            return
                        } else {
                            this.formData1[this.index_y][key1]  = this.formData1[this.table_y][key2]
                            this.formData1[this.table_y][key2] = ''
                        }
                    }
                } else {  //  当拖动的不是请假时
                 console.log('当拖动的不是请假时')
                    if (this.table_x == '2'){   //  拖出的是第二行的
                        console.log('拖出的是第二行的')
                        if (this.index_x == '2') {   // 拉入第二行 （同）
                            console.log('拉入第二行')
                            if(this.formData1[this.index_y][key1].indexOf(',')!='-1'){ //  多个
                               if (this.formData[this.index_y].fp1 == this.formData[this.index_y].fp2) {  // 满了
                                   this.$notify({
                                       title: '警告',
                                       message: '该位置不能放置',
                                       type: 'warning'
                                   });
                                   return;
                               } else {  // 多个
                                   if (this.formData1[this.table_y][key2].indexOf(',')=='-1') { // 拖出自身一个是
                                       this.formData1[this.index_y][key1] = this.formData1[this.index_y][key1]+',' + this.formData1[this.table_y][key2]
                                       this.formData1[this.table_y][key2] = ''
                                   } else {   // 拖出自身是多个
                                       let a = this.formData1[this.table_y][key2].split(',')
                                       let b = []
                                       let c = ''
                                       a.forEach(x =>{
                                           if (x.indexOf(this.textContent)=='-1'){
                                              b.push(x)
                                           } else {
                                               c = x
                                           }
                                       })
                                       this.formData1[this.table_y][key2] = b.join(',')
                                       this.formData1[this.index_y][key1] = this.formData1[this.index_y][key1] + ','+c
                                   }
                               }
                            } else {  //没有或一个
                                if (this.formData1[this.index_y][key1].indexOf('_')!='-1') { //一个

                                    if (this.formData1[this.table_y][key2].indexOf(',')=='-1'){  // 自身一个的时候
                                        this.formData1[this.index_y][key1] = this.formData1[this.index_y][key1]+','+this.formData1[this.table_y][key2]
                                        this.formData1[this.table_y][key2] = ''
                                    } else { // 自身多个的时候
                                        let a = this.formData1[this.table_y][key2].split(',')
                                        let b = []
                                        let c = ''
                                        a.forEach(x =>{
                                            if (x.indexOf(this.textContent)=='-1'){
                                                b.push(x)
                                            }else{
                                                c = x
                                            }
                                        })
                                        this.formData1[this.table_y][key2] = b.join(',')
                                        this.formData1[this.index_y][key1] = this.formData1[this.index_y][key1] + ','+c
                                    }
                                } else { // 没有
                                    if (this.formData1[this.table_y][key2].indexOf(',')=='-1'){  // 自身一个的时候
                                        this.formData1[this.index_y][key1] = this.formData1[this.table_y][key2]
                                        this.formData1[this.table_y][key2] = ''
                                    } else { // 自身多个的时候
                                        this.formData1[this.index_y][key1] = this.formData1[this.table_y][key2]
                                        let a = this.formData1[this.table_y][key2].split(',')
                                        let b = []
                                        a.forEach(x =>{
                                            if (x.indexOf(this.textContent)=='-1'){
                                                b.push(x)
                                            }
                                        })
                                        this.formData1[this.table_y][key2] = b.join(',')
                                    }
                                }

                            }
                        } else{  // 不拉入第二行 （同）
                            console.log('不拉入第二行')
                            if (this.formData1[this.index_y][key1].indexOf('_')!='-1'){ // 拉出发现位置已满
                                this.$notify({
                                    title: '警告',
                                    message: '该位置已存在不能放置',
                                    type: 'warning'
                                });
                                return
                            } else {   //  拉出发现还有位置
                                // 写逻辑
                                if (this.formData1[this.table_y][key2].indexOf(',')!='-1') { // 拖出的东西是多个的
                                    let a = this.formData1[this.table_y][key2].split(',')
                                    let b = []
                                    let c = ''
                                    a.forEach(x =>{
                                        if (x.indexOf(this.textContent)=='-1'){
                                            b.push(x)
                                        } else {
                                            c = x
                                        }
                                    })
                                    this.formData1[this.table_y][key2] = b.join(',')
                                    this.formData1[this.index_y][key1] = c
                                } else {  // 拖出的东西就自己一个
                                    this.formData1[this.index_y][key1]  = this.formData1[this.table_y][key2]
                                    this.formData1[this.table_y][key2] = ''
                                }
                            }

                        }
                    } else {  // 拖出的不是第二行的
                        console.log('拖出的不是第二行的')
                        if (this.index_x == '2') {   // 拉入第二行 （同）
                         console.log('拉入第二行')
                            if (this.formData[this.index_x].fp2 == this.formData[this.index_x].fp1) { // 如果第二行满的情况
                                this.$notify({
                                    title: '警告',
                                    message: '该位置不能放置',
                                    type: 'warning'
                                });
                                return
                            } else {   // 如果不满的情况
                                if (this.formData1[this.index_y][key1].indexOf(',')!='-1') { // 有多个的情况
                                    let a = this.formData1[this.index_y][key1]
                                    let b = this.formData1[this.table_y][key2]
                                    this.formData1[this.index_y][key1] = a + ','+b
                                    this.formData1[this.table_y][key2] = ''
                                } else {  // 一个或没有的情况
                                  if (this.formData1[this.index_y][key1].indexOf('_')!='-1') {  // 有一个的时候
                                      let a = this.formData1[this.index_y][key1]
                                      let b = this.formData1[this.table_y][key2]
                                      this.formData1[this.index_y][key1] = a + ','+b
                                      this.formData1[this.table_y][key2] = ''
                                  } else { // 一个都没有的时候
                                      this.formData1[this.index_y][key1]  = this.formData1[this.table_y][key2]
                                      this.formData1[this.table_y][key2] = ''
                                  }

                                }
                            }
                        } else { // 不拉入第二行 （同）
                         console.log('不拉入第二行')
                          if (this.formData1[this.index_y][key1].indexOf('_')!='-1'){
                                this.$notify({
                                    title: '警告',
                                    message: '该位置已存在不能放置',
                                    type: 'warning'
                                });
                                return
                            } else {
                                this.formData1[this.index_y][key1]  = this.formData1[this.table_y][key2]
                                this.formData1[this.table_y][key2] = ''
                            }
                        }
                    }
                    this.res[key_end]++
                    this.res[key]--
                    this.formData[this.index_y].fp1 ++
                    this.formData[this.table_y].fp1 --
                }


            },
            // 拖动事件
            tableDragstart(e,index) {
                console.log('开始拖动---表格',e.srcElement.attributes.cs.value,index)
                this.table_y = index
                this.table_x = e.srcElement.attributes.cs.value
                console.log('开始坐标x,y',e.srcElement.attributes.cs.value,index)
                this.textContent = e.srcElement.textContent
                console.log(e,e.srcElement.textContent,'查看拖动方块的内容')
                this.index = null
                let str = e.srcElement.textContent
                    str =  str.replace(/\s+/g,"");
                if (str == ''){
                    console.log('拖动的是空白方块');
                    e.preventDefault()
                    return
                }
                if (e.srcElement.textContent.indexOf('请假')!='-1') {
                    this.pd2 = false
                }
                this.pd1 = true
                // 这里可以知道x,y   拖动第二行的  也就知道哪些地方可以落  哪些不能落
                //  这里还要判断下休息的时候  哪些地方能落哪些地方不能落
            },
            tableDragend (e,index){
                console.log(e,index,'拖动结束---表格');
                this.pd1 = false
                this.pd2 = true
            },
            tableDrag(e,index) {  //拖动中
                console.log('拖动中----表格',index);
            },
        }
    };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.content{
    font-size: 14px;
    padding:0 0 0 60px ;
    -moz-user-select: none;
    -webkit-user-select: none;
    -ms-user-select: none;
    user-select: none;
}
.block{
    display: block;
}
.color1{
     background-color: bisque;
     color: cornflowerblue;
 }
.color2{
    background-color: greenyellow;
    color: cornflowerblue;
}
.color3{
    background-color: darkkhaki;
}
.color4{
    background-color: blue;
    color: #ffffff;
}
.bg{
    width: 100px;
    height: 20px;
    display: flex;
    flex-wrap: wrap;
    margin-right: 20px;
    justify-content: center;
    align-content: center;
}
.ys1{
    width: 20px;
    height: 20px;
    display:inline-block;
    position: absolute;
    top:5px;
    left: 0px;
}
.ys2 {
    width: 20px;
    height: 20px;
    display:inline-block;
    position: absolute;
    top:5px;
    left: 150px;
}
.ysShow{
    width: 80%;
    display: flex;
    padding: 10px 20px;
    border: 1px solid #000000;
    flex-wrap: wrap;
    position: relative;
}
.titBg{
    background-color: cornflowerblue;
}
table{
    border-collapse: collapse;
    width: 80%;
}
table tr th {
    min-width: 100px;
    border:1px solid #DCDFE6;
    /*padding: 3px;*/
    height: 20px;
}
table,table tr td { border:1px solid #DCDFE6; }
.addYs {
    height: 20px;
    line-height: 20px;
 }
.ellipse{
       width: 25px;
       height: 20px;
       /*margin: 5px;*/
       background-color: red;
       border-radius: 50% / 50%;
       font-size: 16px;
       font-weight: bold;
       color: #ffffff;
       cursor: pointer;
       /*display: flex;*/
       /*justify-content: center;*/
       /*align-content: center;*/
     }
.ysShow_close{
    position: absolute;
    top: 0px;
    right: 0px;
}

 .dw{
        width: 300px;
        height: 30px;
        position: relative;
        display: inline-block;
    }
    .text1{
     position: absolute;
     top:5px;
     left:30px;
    }
    .text2{
     position: absolute;
     top:5px;
     left: 178px;
    }
    .b_hover{
        border: 2px solid aquamarine !important;
    }
    .b_fff{
        border: 2px solid #ffffff;
    }
</style>
