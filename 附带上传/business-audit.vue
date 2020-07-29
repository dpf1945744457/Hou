<template>
  <div class="container">
    <div class="container_on">
      <div class="on_title">
        <p>查询条件</p>
      </div>
      <el-form
        :model="formInline"
        ref="dengmiQueryForm"
        label-width="100px"
        class="demo-ruleForm"
        size="mini"
      >
        <el-row>
          <el-col :span="3">
            <el-form-item label="企业名称">
              <el-input v-model="formInline.merchantName"></el-input>
            </el-form-item>
          </el-col>

          <el-col :span="3">
            <el-form-item label="法人姓名">
              <el-input v-model="formInline.legalPersonName"></el-input>
            </el-form-item>
          </el-col>

          <el-col :span="3">
            <el-form-item label="审核人">
              <el-input v-model="formInline.reviewerName"></el-input>
            </el-form-item>
          </el-col>

          <el-col :span="3">
            <el-form-item label="审核状态">
              <el-select
                v-model="formInline.status"
                placeholder="请选择"
              >
                <el-option
                  v-for="item in options"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                >
                </el-option>
              </el-select>
            </el-form-item>
          </el-col>

          <el-col :span="8">
            <el-form-item label="创建时间">
              <div class="two_nums">
                <div>
                  <el-date-picker
                    v-model="formInline.createTimeStar"
                    type="datetime"
                    placeholder="选择日期时间"
                    style="width:80%"
                  >
                  </el-date-picker>
                </div>
                <div> - </div>
                <div>

                  <el-date-picker
                    v-model="formInline.createTimeEnd"
                    type="datetime"
                    placeholder="选择日期时间"
                    style="width:80%"
                  >
                  </el-date-picker>
                </div>
              </div>
            </el-form-item>
          </el-col>

          <el-col :span="4">
            <el-button
              type="primary"
              size="medium"
              icon="el-icon-search"
              style="margin-right:30px"
              @click="onSubmit('formInline')"
              round
            >查询</el-button>
          </el-col>
        </el-row>

      </el-form>
    </div>

    <div class="container_tw">
      <div class="tw_title">
        <p>系统列表</p>
        <!--右边按钮  -->
        <div class="tools-right">
          <!-- <el-button
            type="primary"
            class="tools-button"
            @click="addSystem"
          >
            <i class="el-icon-circle-plus-outline"></i> 添加
          </el-button> -->
        </div>
        <!--<el-button type="primary" icon="el-icon-circle-plus" size="small">搜索</el-button>-->
      </div>
      <div class="tw_form">
        <!-- <el-table
          v-if="statusList == 1"
          :data="merchantList"
          style="width: 100%"
          class="table"
          height="calc(100vh - 450px)"
        > -->
        <el-table
          :data="merchantList"
          style="width: 100%"
          class="table"
          height="calc(100vh - 450px)"
        >
          <!-- <el-table-column prop="id" label="ID" ></el-table-column> -->

          <el-table-column
            type="index"
            label="序号"
          ></el-table-column>
          <el-table-column
            prop="merchantName"
            label="企业名称"
            align="center"
          ></el-table-column>
          <el-table-column
            prop="legalPersonName"
            label="法人姓名"
            align="center"
          ></el-table-column>
          <el-table-column
            prop="creatorName"
            label="提交人"
            align="center"
          ></el-table-column>
          <el-table-column
            prop="status"
            label="审核状态"
            align="center"
          ></el-table-column>
          <el-table-column
            prop="createTime"
            label="创建时间"
            align="center"
          ></el-table-column>
          <el-table-column
            prop="reviewerName"
            label="审核人"
            align="center"
          ></el-table-column>
          <el-table-column
            prop="reviewTime"
            label="审核时间"
            align="center"
          ></el-table-column>
          <el-table-column
            label="操作"
            width="400"
            align="center"
          >
            <template slot-scope="scope">
              <el-button
                type="primary"
                size="small"
                @click="auditShop(scope.row)"
              >审核</el-button>
              <el-button
                type="success"
                size="small"
                @click="details(scope.row)"
              >详情</el-button>
            </template>
          </el-table-column>
        </el-table>
      </div>
      <div
        class="block"
        style="text-align:center; margin-top:2em"
      >
        <el-pagination
          background
          :current-page="pageNum"
          :page-sizes="[5,10,15,20]"
          :page-size="pageSize"
          layout="total, sizes, prev, pager, next, jumper"
          :total="total"
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
        ></el-pagination>
        <!-- <el-pagination
          background
          :current-page="pageNum"
          :page-sizes="[5,10,15,20]"
          :page-size="pageSize"
          layout="total, sizes, prev, pager, next, jumper"
          :total="total"
        ></el-pagination> -->
      </div>
      <!-- 弹出框 -->
      <el-dialog
        :title="dltitle"
        :visible.sync="dialogVisible"
        width="800px"
      >
        <el-form
          ref="form"
          :model="form"
          label-width="140px"
          label-position="left"
          :rules="rules"
        >
          <el-form-item
            label="企业名称"
            prop="merchantName"
          >
            <el-input
              v-model="form.merchantName"
              placeholder="请输入企业名称"
            ></el-input>
          </el-form-item>
          <el-form-item
            label="企业税号"
            prop="merchantTaxCode"
          >
            <el-input
              v-model="form.merchantTaxCode"
              placeholder="请输入企业税号"
            ></el-input>
          </el-form-item>
          <el-form-item
            label="法人姓名"
            prop="legalPersonName"
          >
            <el-input
              v-model="form.legalPersonName"
              placeholder="请输入法人姓名"
            ></el-input>
          </el-form-item>
          <el-form-item
            label="法人身份证"
            prop="legalPersonIdCard"
          >
            <el-input
              v-model="form.legalPersonIdCard"
              placeholder="请输入法人身份证"
            ></el-input>
          </el-form-item>
          <el-form-item
            label="工商注册地址"
            prop="companyAddress"
          >
            <el-input
              v-model="form.companyAddress"
              placeholder="请输入工商注册地址"
            ></el-input>
          </el-form-item>
          <el-form-item
            label="经营范围"
            prop="businessScope"
          >
            <el-input
              v-model="form.businessScope"
              placeholder="例如：计算机软件、计算机信息系统软件开发"
            ></el-input>
          </el-form-item>
          <el-form-item label="企业经营类别">
            <el-checkbox-group v-model="form.businessType">
              <el-checkbox label="1">电商商户</el-checkbox>
              <el-checkbox label="2">电商平台</el-checkbox>
              <el-checkbox label="3">电商代理企业</el-checkbox>
            </el-checkbox-group>
          </el-form-item>
          <el-form-item
            label="联系人"
            prop="contactName"
          >
            <el-input
              v-model="form.contactName"
              placeholder="请输入联系人"
            ></el-input>
          </el-form-item>
          <el-form-item
            label="联系电话"
            prop="contactPhone"
          >
            <el-input
              v-model="form.contactPhone"
              placeholder="请输入联系电话"
            ></el-input>
          </el-form-item>
          <el-form-item
            label="联系地址"
            prop="contactAddress"
          >
            <el-input
              v-model="form.contactAddress"
              placeholder="请输入联系地址"
            ></el-input>
          </el-form-item>
          <el-form-item
            label="联系人邮箱"
            prop="contactMail"
          >
            <el-input
              v-model="form.contactMail"
              placeholder="请输入联系地址"
            ></el-input>
          </el-form-item>
          <el-form-item
            label="网站名称"
            prop="webSiteName"
          >
            <el-input
              v-model="form.webSiteName"
              placeholder="请输入网站名称"
            ></el-input>
          </el-form-item>
          <el-form-item
            label="网址"
            prop="webSiteAddress"
          >
            <el-input
              v-model="form.webSiteAddress"
              placeholder="请输入网址"
            ></el-input>
          </el-form-item>
          <el-form-item
            label="经营商品"
            prop="businessGoods"
          >
            <el-input
              v-model="form.businessGoods"
              placeholder="例如：服装、鞋帽、化妆品、母婴产品"
            ></el-input>
          </el-form-item>
          <el-form-item label="海关通道">
            <el-checkbox-group v-model="form.customsChannelsIds">
              <el-checkbox label="深圳海关电子口岸"></el-checkbox>
              <el-checkbox label="广州海关电子口岸"></el-checkbox>
            </el-checkbox-group>
          </el-form-item>

          <el-form-item
            label="上传照片"
            prop="busLicensePic"
          >
            <el-upload
              class="upload-demo"
              action="https://jsonplaceholder.typicode.com/posts/"
              :on-preview="handlePreview"
              :on-remove="handleRemove"
              :before-remove="beforeRemove"
              multiple
              :limit="3"
              :on-exceed="handleExceed"
              :file-list="fileList"
            >
              <el-button
                size="small"
                type="primary"
              >点击上传</el-button>
              <div
                slot="tip"
                class="el-upload__tip"
              >
                1、支持的图片格式：BMP、TIFF、GIF、PNG、JPEG <br>

                2、文件大小应该小于100KB <br>

                3、可一次选择多张照片上传、上传的图片数量最多20张

                文本框、必填、由1-18个英文、数字字符组成 <br>

                4、可上传法人身份证正反面、营业热照等 <br></div>
            </el-upload>

          </el-form-item>

          <el-form-item
            label="审核状态"
            prop="status"
             v-if="dltitle == '商家详情' "
          >
            <p>{{form.status}}</p>
          </el-form-item>
          <el-form-item
            label="审核时间"
            prop="reviewTime"
            v-if="dltitle == '商家详情' "
          >
            <el-input v-model="form.reviewTime"></el-input>
          </el-form-item>
          <el-form-item
            label="审核人"
            prop="reviewerName"
            v-if="dltitle == '商家详情'"
          >
            <el-input v-model="form.reviewerName"></el-input>
          </el-form-item>
          <el-form-item
            label="审核备注"
            prop="reviewRemark"
          >
            <el-input
              type="textarea"
              :rows="4"
              v-model="form.reviewRemark"
            >
            </el-input>
          </el-form-item>

          <el-form-item>
            <el-button
              v-if="dltitle =='商家审核'"
              type="success"
              @click="pass('form', '2')"
            >审核通过</el-button>
            <el-button
              v-if="dltitle =='商家审核'"
              type="success"
              @click="pass('form' ,3)"
            >不通过并结束</el-button>
            <el-button
              v-if="dltitle =='商家审核'"
              type="success"
              @click="pass('form', 1)"
            >退回修改</el-button>
          </el-form-item>
        </el-form>
      </el-dialog>
    </div>
    <!-- 分页 -->
  </div>
</template>

<script>
import {
  getList,
  addMer,
  editMer,
  getDetailMer,
  auditMer,
  uploadFile,
} from '@/api/business'
import { username, validEmail, validphone } from '@/utils/validate'
import { channelList } from '@/api/commodity-record'
export default {
  name: 'PermissionGroup',
  data() {
    var validateusername = (rule, value, callback) => {
      if (value == '' || value == undefined) {
        callback(new Error('请输入登录账号'))
      } else if (!username(value)) {
        callback(new Error('只能包含字母和数字'))
      }
      callback()
    }
    var validateemail = (rule, value, callback) => {
      if (value == '' || value == undefined) {
        callback(new Error('请输入邮箱'))
      } else if (!validEmail(value)) {
        callback(new Error('请输入正确的邮箱'))
      }
      callback()
    }
    var phone = (rule, value, callback) => {
      if (value == '' || value == undefined) {
        callback(new Error('请输入手机号码'))
      } else if (!validphone(value)) {
        callback(new Error('请输入正确的手机号码'))
      }
      callback()
    }
    return {
      formInline: {},
      dialogVisible: false,
      fileList: [
        {
          name: 'food.jpeg',
          url:
            'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100',
        },
        {
          name: 'food2.jpeg',
          url:
            'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100',
        },
      ],
      pageNum: 1,
      pageSize: 5,
      total: 0,
      value1: '',
      MenuList: [],
      merchantList: [], // 商家
      statusList: [], //状态
      customsChanneList: [], //海关通道

      statusList: [], //状态
      deptCodes: [],
      dengmiQueryForm: '',
      options: [],
      businessType: [],
      creatorId: [],
      value: '',
      dltitle: '',

      addOrEdit: 'add',
      form: {
        // businessGoods: '', // 经营商品
        // businessScope: '', // 业务范围
        // businessType: [], // 业经营类型
        // companyAddress: '', // 工商注册地址
        // contactAddress: '', // 联系地址
        // contactMail: '', // 联系邮箱
        // contactName: '', // 联系人名称
        // contactPhone: '', // 联系电话

        // // createTime: '', // 创建时间
        // // creatorId: '', // 创建人id
        // // creatorName: '', // 创建人名称

        // customsChannelsIds: [], // 海关通道 关联customs_channels中的id

        // id: '', // ID

        // legalPersonIdCard: '', // 法人身份证
        // legalPersonName: '', // 法人姓名

        // // merchantCode: '', // 商户编码 格式字母加数字

        // merchantName: '', // 企业名称
        // merchantTaxCode: '', // 企业税号

        // // pics: '', // 图片集合
        // reviewRemark: '', // 审核备注
        // reviewTime: '', // 审核时间
        // reviewerId: '', // 审核编号
        // reviewerName: '', // 审核人名称
        // status: '', // 商家状态
        // // updateTime: '', // 修改时间

        // webSiteAddress: '', // 网站地址
        // webSiteName: '', // 网站名称

        id: '',
        reviewRemark: '',
        reviewTime: '',
        reviewerId: '',
        reviewerName: '',
        status: '',
      },
      defaultProps: {
        children: 'roleList',
        label: 'name',
      },
      dialogVisibleuser: false,
      rules: {
        merchantName: [
          {
            // 企业名称
            required: true,
            message: '必填、由1-50个中文、英文、数字及字符组成',
            trigger: 'blur',
          },
        ],
        merchantTaxCode: [
          {
            // 企业税号
            required: true,
            message: '必填、由1-18个英文、数字组成',
            trigger: 'blur',
          },
        ],
        legalPersonName: [
          {
            // 法人姓名
            required: true,
            message: '必填、由1-32个字符组成',
            trigger: 'blur',
          },
        ],
        legalPersonIdCard: [
          {
            // 法人身份证
            required: true,
            message: '必填、由1-18个英文、数字字符组成',
            trigger: 'blur',
          },
        ],
        companyAddress: [
          {
            // 工商注册地址
            required: true,
            message: '必填，由1-150个字符组成',
            trigger: 'blur',
          },
        ],
        businessScope: [
          {
            // 业务范围
            required: true,
            message: '必填，由1-150个字符组成',
            trigger: 'blur',
          },
        ],
        businessType: [
          {
            // 业经营类型
            required: true,
            message: '必选',
            trigger: 'change',
          },
        ],
        contactName: [
          {
            // 联系人名称
            required: true,
            message: '必填、由1-32个字符组成',
            trigger: 'blur',
          },
        ],
        contactPhone: [
          {
            // 联系电话
            required: true,
            message: '必填、由1-20个数字字符组成',
            trigger: 'blur',
          },
        ],
        contactAddress: [
          {
            // 联系电话
            required: true,
            message: '选填、由1-150个字符组成',
            trigger: 'blur',
          },
        ],
        contactMail: [
          {
            //联系邮箱
            required: false,
            message: '选填',
            trigger: 'blur',
          },
        ],
        webSiteName: [
          {
            //网站名称
            required: true,
            message: '必填、由1-30个字符组成',
            trigger: 'blur',
          },
        ],
        webSiteAddress: [
          {
            // 网站地址
            required: false,
            message: '请输入小区名称',
            trigger: 'blur',
          },
        ],

        businessGoods: [
          {
            // 经营商品
            required: true,
            message: '必填、由1-150个字符组成',
            trigger: 'blur',
          },
          //          { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
        ],
        customsChannelsIds: [
          {
            // 海关通道
            required: true,
            message: '必填',
            trigger: 'change',
          },
          //          { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
        ],
      },
    }
  },
  created() {
    // this.userlists()
    // this.getusergetDeptTree()
    this.getListMerchant()
    this.getChannelList()
    // this.fileUp()
  },
  methods: {
    // formatEnable(row, column) {
    //   return row.enable == true ? '启用' : '停用'
    // },
    getListMerchant() {
      getList({
        merchantName: this.formInline.merchantName,
        legalPersonName: this.formInline.legalPersonName,
        reviewerName: this.formInline.reviewerName,
        status: this.formInline.status,
        createTimeStar: this.formInline.createTimeStar,
        createTimeEnd: this.formInline.createTimeEnd,
        pageNum: this.pageNum,
        pageSize: this.pageSize,
      })
        .then((response) => {
          if (response.code == '200') {
            // Merchant list
            console.log(response, '返回的商家信息参数')
            this.merchantList = response.data.dataList
            console.log(this.merchantList, '返回的参数')
            // debugger
            for (let item = 0; item < this.merchantList.length; item++) {
              console.log(this.merchantList[item].status, '6666666666')
              this.statusList = this.merchantList[item].status
            }
            this.total = response.data.total
          }
        })
        .catch((error) => {
          // this.$message.error(error.errmsg);
        })
    },
    onSubmit() {
      this.getListMerchant()
    },
    // 海关通道数组
    cancel(formName) {
      this.$refs[formName].clearValidate()
      this.dialogVisible = false
    },
    auditShop(row) {
      this.form = {
        // businessGoods: '', // 经营商品
        // businessScope: '', // 业务范围
        // businessType: [], // 业经营类型
        // companyAddress: '', // 工商注册地址
        // contactAddress: '', // 联系地址
        // contactMail: '', // 联系邮箱
        // contactName: '', // 联系人名称
        // contactPhone: '', // 联系电话
        // // createTime: '', // 创建时间
        // // creatorId: '', // 创建人id
        // // creatorName: '', // 创建人名称
        // customsChannelsIds: [], // 海关通道 关联customs_channels中的id
        // // id: '', // ID
        // legalPersonIdCard: '', // 法人身份证
        // legalPersonName: '', // 法人姓名
        // // merchantCode: '', // 商户编码 格式字母加数字
        // merchantName: '', // 企业名称
        // merchantTaxCode: '', // 企业税号
        // // pics: '', // 图片集合
        // // reviewRemark: '', // 审核备注
        // // reviewTime: '', // 审核时间
        // // reviewerId: '', // 审核编号
        // // reviewerName: '', // 审核人名称
        // // status: '', // 商家状态
        // // updateTime: '', // 修改时间
        // webSiteAddress: '', // 网站地址
        // webSiteName: '', // 网站名称
        id: '',
        reviewRemark: '',
        reviewTime: '',
        reviewerId: '',
        reviewerName: '',
        status: '',
      }
      this.form = row
      this.dltitle = '商家审核'
      this.dialogVisible = true
    },
    // 详情
    details(row) {
      this.dltitle = '商家详情'
      console.log(row, '666668888888888888888啊啊')
      const params = {
        id: row.id,
      }
      getDetailMer(params).then((response) => {
        this.form = response.data
        this.dialogVisible = true
        console.log(response, '详情666666666666666666666')
      })
    },
    // 编辑
    audit(row) {
      this.dltitle = '商家审核'
      this.form = row
      this.dialogVisible = true
    },

    // 通过
    pass(formName, num) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          console.log(this.form)
          auditMer(this.form, num)
            .then((response) => {
              console.log(response, '这是通过的信息参数+++++++++++++++')
              this.dialogVisible = false
              this.getListMerchant()
            })
            .catch((error) => {
              console.log(error)
            })
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },

    // pass(formName){
    //   const params = {
    //     id: this.id,
    //     reviewRemark: this.reviewRemark,
    //     reviewTime: this.reviewTime,
    //     reviewerId: this.reviewerId,
    //     reviewerName: this.reviewerName,
    //     status: 2
    //   }
    //   auditMer(params).then((response => {
    //     this.form = response.data
    //   }))
    // },
    // 海关下拉表
    getChannelList() {
      channelList().then((response) => {
        this.customsChanneList = response.data
      })
    },

    // 审核商家
    // pass(row){
    //   console.log(row, '6888888888888886')
    // const params = {
    //     id: row.id,
    //     reviewRemark: this.reviewRemark,
    //     reviewTime: this.reviewTime,
    //     reviewerId: this.reviewerId,
    //     reviewerName: this.reviewerName,
    //     status: this.status,
    //   }
    //   auditMer(params).then(( response => {
    //     console.log(变更, '66666666666666666')
    //   }))
    // },

    // 图片上传
    fileUp() {
      const params = {
        bucketName: this.bucketName,
        expires: this.expires,
        perfixName: this.perfixName,
        upfile: this.upfile,
      }
      uploadFile(params).then((response) => {
        console.log(response, '图片上传')
      })
    },

    // 图片
    handleRemove(file, fileList) {
      console.log(file, fileList)
    },
    handlePreview(file) {
      this.fileUp()
    },
    handleExceed(files, fileList) {
      this.$message.warning(
        `当前限制选择 3 个文件，本次选择了 ${files.length} 个文件，共选择了 ${
          files.length + fileList.length
        } 个文件`
      )
    },
    beforeRemove(file, fileList) {
      return this.$confirm(`确定移除 ${file.name}？`)
    },
    handleSizeChange(val) {
      this.pageSize = val
      this.getListMerchant()
    },
    handleCurrentChange(val) {
      this.pageNum = val
      this.getListMerchant()
    },
  },
}
</script>

<style lang='scss' scope>
.container {
  padding: 20px;
}
.container_on {
  // padding: 20px;
  height: 150px;
  background: white;
  .on_title {
    margin-bottom: 20px;
    height: 50px;
    display: flex;
    justify-content: space-between;
    box-sizing: border-box;
    padding: 0 10px;
    border-bottom: 1px solid rgb(231, 234, 236);
    & > p:nth-child(1) {
      color: rgb(104, 107, 109);
      font-size: 14px;
      font-weight: bold;
    }
    & > p:nth-child(2) {
      color: rgb(1, 123, 255);
      font-size: 14px;
    }
    & > p:nth-child(2):hover {
      text-decoration: underline;
      cursor: pointer;
    }
  }
  .on_input {
    .el-row {
      display: flex;
      justify-content: space-around;
    }
  }
  .row {
    margin-bottom: 20px;
    .col_left {
      & > span {
        color: #606266;
        font-weight: bold;
        font-size: 14px;
      }
    }
  }
  .two_nums {
    // & > div {
    //   display: inline-block;
    // }
    display: flex;
    justify-content: space-around;
    align-items: center;
    & > div:nth-child(1) {
      width: 45%;
    }
    & > div:nth-child(2) {
      width: 8%;
    }
    & > div:nth-child(3) {
      width: 45%;
    }
  }
}
// span {
//   display: block;
//   text-align: right;
//   padding-right: 5px;
// }

.container_tw {
  margin-top: 30px;
  .tw_title {
    height: 50px;
    background: white;
    display: flex;
    justify-content: space-between;
    align-items: center;
    box-sizing: border-box;
    padding: 0 20px;
    border-bottom: 1px solid rgb(231, 234, 236);
    & > p {
      color: rgb(104, 107, 109);
      font-size: 14px;
      font-weight: bold;
    }
    .el-button:hover {
      background: rgb(27, 179, 148);
    }
  }
  .tw_form {
    box-sizing: border-box;
    .el-table {
      box-sizing: border-box;
      padding: 0 20px;
    }
  }
}

.pagination-container {
  background: #ffffff;
  padding: 10px 0;
  margin: 20px;
}
</style>
