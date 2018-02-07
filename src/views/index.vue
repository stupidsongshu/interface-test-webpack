<template>
  <div class="index">
    <div class="commonParam">
        <button class="btn btn-primary">url</button>
        <input type="text" v-model="url" class="form-control" style="min-width: 300px;">
    </div>
    <div class="commonParam">
        <button class="btn btn-primary">call</button>
        <input type="text" v-model="call" class="form-control">
    </div>
    <div class="commonParam">
        <button class="btn btn-primary">ua</button>
        <input type="text" v-model="ua" class="form-control">
    </div>
    <div class="commonParam">
        <button class="btn btn-primary">signKey</button>
        <input type="text" v-model="signKey" class="form-control">
    </div>

    <tree :node="root" length="1" ref="treeParent"></tree>

    <div style="margin: 35px;">
        <button class="btn btn-success" @click="preview">Preview</button>
        <button class="btn btn-success" @click="test">Test</button>
    </div>

    <div class="container-fluid">
        <div class="row">
            <!-- <div class="col-sm-6">
                <div class="text-center">原始数据</div>
                <pre>{{originData}}</pre>
            </div> -->
            <div class="col-sm-12" v-if="previewParamsData">
                <div class="text-center">预览请求参数</div>
                <pre>{{previewParamsData}}</pre>
            </div>
        </div>
    </div>

    <div style="width: 100%;margin: 20px 0;border: 1px dotted #000;"></div>

    <loading v-show="isLoading"></loading>

    <div class="container-fluid" v-if="httpResult">
        <div class="row">
            <div class="col-sm-12">
                <div class="text-center">请求响应<span v-show="httpResultStatus">成功</span><span v-show="!httpResultStatus">失败</span></div>
                <pre :class="{httpSuccess: httpResultStatus, httpFail: !httpResultStatus}">{{httpResult}}</pre>
            </div>
        </div>
    </div>
  </div>
</template>

<script>
import md5 from 'blueimp-md5'
import axios from 'axios'
import tree from '../components/tree'
import loading from '../components/loading'

export default {
  name: 'Index',
  data() {
    return {
      root: {
        name: 'dynamic params',
        children: [
          {
            name: 'mobileNo',
            val: '13720314834',
            children: []
          }
        ]
      },
      url:'http://xfjr.ledaikuan.cn:9191/activity/i/h',
      call: 'CashBackVoucher.list',
      ua: 'Ledaikuan_H5_Sign',
      signKey: '68352e6b616875616e77616e672e636f6d',
      // 生成原始参数数据格式
      originData: null,
      // 预览参数数据结构
      previewParamsData: null,
      httpResultStatus: false,
      httpResult: '',
      isLoading: false
    }
  },
  components: {
    tree,
    loading
  },
  mounted() {
    this.originData = this.$refs.treeParent.node.children
    this.preview()
  },
  methods: {
    // preview1() {
    //     let object = {}
    //     let count = 0
    //     // let tmpArr = []

    //     function iterator(arr) {
    //         arr.forEach((item0, index0, arr0) => {
    //             object[item0.name] = {}

    //             if (item0.children.length === 0) {
    //                 object[item0.name] = item0.val
    //             } else if (item0.children.length > 0) {
    //                 // tmpArr['item'+count] = 
    //                 item0.children.forEach((itemChild1, indexChild1, itemArr1) => {
    //                     count++
    //                     object[item0.name][itemChild1.name] = {}

    //                     if (itemChild1.children.length === 0) {
    //                         object[item0.name][itemChild1.name] = itemChild1.val
    //                     } else if (itemChild1.children.length > 0) {
    //                         itemChild1.children.forEach((itemChild2, indexChild2, itemArr2) => {
    //                             count++
    //                             object[item0.name][itemChild1.name][itemChild2.name] = {}

    //                             if (itemChild2.children.length === 0) {
    //                                 object[item0.name][itemChild1.name][itemChild2.name] = itemChild2.val
    //                             } else if (itemChild2.children.length > 0) {
    //                                 itemChild2.children.forEach((itemChild3, indexChild3, itemArr3) => {
    //                                     if (itemChild3.children.length === 0) {
    //                                         object[item0.name][itemChild1.name][itemChild2.name][itemChild3.name] = itemChild3.val
    //                                     } else if (itemChild3.children.length > 0) {
    //                                         // ...继续重复
    //                                         count++
    //                                     }
    //                                 })
    //                             }
    //                         })
    //                     }
    //                 })
    //             }
    //         })

    //         return object
    //     }

    //     let tmpData = iterator(this.originData)
    //     this.previewParamsData = tmpData
    //     console.log(count)
    // },
    // preview2() {
    //     let originData = this.$refs.treeParent.$children
    //     let object = {}

    //     function iterator(arr) {
    //         arr.forEach((item, index, arr) => {
    //             // root的直接子节点
    //             if (item.node.children.length === 0) {
    //                 object[item.node.name] = item.node.val
                    
    //             } else if (item.node.children.length > 0) {
    //                 iterator(item.$children)
    //             }
    //         })

    //         return object
    //     }

    //     let info = iterator(originData)
    //     console.log(info)
    // },
    preview() {
      const parse = arr => arr.reduce((obj, { name, val, children }) => ({
            ...obj,
            ...{
            [name]: children.length === 0
              ? val
              : parse(children)
            }
          })
      , {})

      let ua = this.ua
      let call = this.call
      let timestamp = new Date().getTime()
      let signKey = this.signKey
      let sign = md5(ua + '&' + call + '&' + timestamp + '&' + signKey)

      let commonParams = {
        call: call,
        ua: ua,
        sign: sign,
        timestamp: timestamp
      }

      let dynamicParams = {
        args: parse(this.originData)
      }

      this.previewParamsData = Object.assign(commonParams, dynamicParams)

      return this.previewParamsData
    },
    test() {
      let paramsObj = this.preview()
      let paramString = JSON.stringify(paramsObj)

      this.isLoading = true

      axios({
        method: 'post',
        url: this.url,
        data: paramString
      }).then(res => {
        this.isLoading = false
        console.log(res)
        this.httpResultStatus = true
        this.httpResult = res.data
      }).catch(err => {
        this.isLoading = false
        console.log(err)
        this.httpResultStatus = false
        this.httpResult = err
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.index {
  padding-top: 20px;
}
.form-control {
  display: inline-block !important;
  width: auto !important;
}
.number {
  border: 1px solid #f00;
}
.commonParam {
  margin: 4px 35px;
}
.treeWrapper {
  position: relative;
  margin: 4px 35px;
}
.treeWrapper::before {
  content: '';
  position: absolute;
  top: 40px;
  left: 17px;
  height: calc(100% - 40px);
  border-left: 1px solid #ccc;
}

.httpSuccess {
  color: #000;
}
.httpFail {
  color: #f00;
}
</style>
