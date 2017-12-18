<style lang="less">
    @import './order-details.less';
</style>
<template>
  <div style="padding:15px;background:#FFF;overflow:hidden">
    <row style="margin-top:10px">
        <Col span="6">
            选择查询时间段 <DatePicker type="daterange" placement="bottom-start" placeholder="请选择查询时间段" style="width: 200px"></DatePicker>
         </Col>
         <Col span="4">
            分部名称：
            <Select v-model="branch" style="width:120px">
              <Option v-for="item in cityList" :value="item.value" :key="item.value">{{ item.label }}</Option>
            </Select>
          </Col>
        <Col span="4">
             子分部名称：
            <Select v-model="subdivision" style="width:120px">
                <Option v-for="item in cityList" :value="item.value" :key="item.value">{{ item.label }}</Option>
            </Select>
         </Col>
         <Col span="4">
              &nbsp;理财师：
            <Select v-model="model4" style="width:120px">
                <Option v-for="item in cityList" :value="item.value" :key="item.value">{{ item.label }}</Option>
            </Select>
         </Col>
         <Col span="3">
            <Button type="primary" icon="ios-search">Search</Button>
         </Col>
    </row>

    <h2 style="margin-top:40px">总数据</h2>
    <div style="width:600px;margin-top:10px;">
      <Table border :columns="columns1" :data="data1"></Table>
    </div>
    <div  style="margin-top:40px;">
      <Col span="2">
        <h2>订单详情</h2>
      </Col>
      <Col span="2">
        <Button type="primary" icon="ios-cloud-download" @click="exportData(1)">导出</Button>
      </Col>
    </div>
    <div style="clear:both;padding-top:20px;">
      <Table border :columns="columns8" :data="data7" size="small" ref="table"></Table>
    </div>
    <div style="margin-top:10px;float:right">
      <Page :total="40" size="small" show-elevator show-sizer></Page>
    </div>
    <div style="margin-top:10px;float:left;line-height:22px;">显示？？条到？？条记录，总共{{total}}条记录。</div>
  </div>
</template>
<script>
import util from 'utils';
    export default {
        data () {
            return {
                cityList: [
                    {
                        value: 'New York',
                        label: 'New York'
                    },
                    {
                        value: 'London',
                        label: 'London'
                    },
                    {
                        value: 'Sydney',
                        label: 'Sydney'
                    },
                    {
                        value: 'Ottawa',
                        label: 'Ottawa'
                    },
                    {
                        value: 'Paris',
                        label: 'Paris'
                    },
                    {
                        value: 'Canberra',
                        label: 'Canberra'
                    }
                ],
                branch: '',
                subdivision: '',
                model4: '',
                total:'',
                columns1: [
         {
             title: '订单笔数',
             key: 'name'
         },
         {
             title: '交易额（元）',
             key: 'age'
         },
         {
             title: '年华交易总额（元）',
             key: 'address'
         }
     ],
     data1: [
         {
             name: 'John Brown',
             age: 18,
             address: 'New York No. 1 Lake Park',
             date: '2016-10-03'
         }
     ],
     columns8: [
         {
             "title": "序号",
             "type": "index",
            "fixed": "left",
             "width": 100
         },
         {
             "title": "用户名称",
             "key": "show",
             "width": 150,
           //  "sortable": true,
           //  filters: [
             //     {
             //         label: 'Greater than 4000',
             //         value: 1
             //     },
             //     {
             //         label: 'Less than 4000',
             //         value: 2
             //     }
             // ],
             //filterMultiple: false,
             // filterMethod (value, row) {
             //     if (value === 1) {
             //         return row.show > 4000;
             //     } else if (value === 2) {
             //         return row.show < 4000;
             //     }
             // }
         },
         {
             "title": "订单号",
             "key": "weak",
             "width": 150,
         //    "sortable": true
         },
         {
             "title": "产品名称",
             "key": "signin",
             "width": 150,
         //    "sortable": true
         },
         {
             "title": "产品期限",
             "key": "click",
             "width": 150,
         //    "sortable": true
         },
         {
             "title": "交易金额",
             "key": "active",
             "width": 150,
         //    "sortable": true
         },
         {
             "title": "投资年华金额",
             "key": "day7",
             "width": 150,
         //    "sortable": true
         },
         {
             "title": "投资时间",
             "key": "day30",
             "width": 150,
         //    "sortable": true
         },
         {
             "title": "理财师",
             "key": "tomorrow",
             "width": 150,
         //    "sortable": true
         },
         {
             "title": "子分部名称",
             "key": "day",
             "width": 150,
       //      "sortable": true
         },
         {
             "title": "分部名称",
             "key": "week",
             "width": 150,
         //    "sortable": true
         },
         {
             "title": "Month Active",
             "key": "month",
             "width": 150,
         //    "sortable": true
         }
     ],
     data7: [],

            }
        },
        methods: {
        exportData (type) {
            if (type === 1) {
                this.$refs.table.exportCsv({
                    filename: 'The original data'
                });
            } else if (type === 2) {
                this.$refs.table.exportCsv({
                    filename: 'Sorting and filtering data',
                    original: false
                });
            } else if (type === 3) {
                this.$refs.table.exportCsv({
                    filename: 'Custom data',
                    columns: this.columns8.filter((col, index) => index < 4),
                    data: this.data7.filter((data, index) => index < 4)
                });
            }
        },
        async  getData (){

          // let result = await util.ajax.get('/list');
           let result = await util.ajax.post('/order');
           if(result.data.success){
                this.data7= result.data.data;
                this.total= result.data.total;
           }else {

           }

        }
    },
    created(){
        this.getData();
    }

    }
</script>
