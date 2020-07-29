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
          <el-col :span="5">
            <el-form-item label="企业名称">
              <el-input v-model="formInline.merchantName"></el-input>
            </el-form-item>
          </el-col>

          <el-col :span="5">
            <el-form-item label="法人姓名">
              <el-input v-model="formInline.legalPersonName"></el-input>
            </el-form-item>
          </el-col>

          <el-col :span="5">
            <el-form-item label="审核人">
              <el-input v-model="formInline.reviewerName"></el-input>
            </el-form-item>
          </el-col>

          <el-col :span="4">
            <el-form-item label="海关通道">
              <el-select
                v-model="formInline.customsChannelsIds"
                placeholder="请选择海关通道"
              >
                <el-option
                  v-for="item in customsChanneList"
                  :key="item.id"
                  :label="item.channelName"
                  :value="item.id"
                >
                </el-option>
              </el-select>
            </el-form-item>
          </el-col>

          <el-col :span="4">
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
        </el-row>

        <el-row
          type='flex'
          align='middle'
          class="row"
        >
          <el-col
            :span="3"
            class="col_left"
            style="margin-left:30px"
          >
            <span>创建时间:</span>
          </el-col>
          <el-col :span="9">
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
          </el-col>
          <el-col
            :span="3"
            class="col_left"
          >
            <span>审核时间:</span>
          </el-col>
          <el-col :span="9">
            <div class="two_nums">
              <div>
                <el-date-picker
                  v-model="formInline.reviewTimeStar"
                  type="datetime"
                  placeholder="选择日期时间"
                  style="width:80%"
                >
                </el-date-picker>
              </div>
              <div> - </div>
              <div>
                <el-date-picker
                  v-model="formInline.reviewTimeEnd"
                  type="datetime"
                  placeholder="选择日期时间"
                  style="width:80%"
                >
                </el-date-picker>
              </div>
            </div>
          </el-col>
          <el-col :span="8">
            <el-button
              type="primary"
              size="medium"
              icon="el-icon-search"
              style="margin-right:30px"
              @click="onSubmit('formInline')"
              round
            >查询</el-button>
            <el-button
              type="primary"
              size="medium"
              icon="el-icon-search"
              @click="add"
              round
            >新增商家</el-button>
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
            prop="contactName"
            label="联系人"
            align="center"
          ></el-table-column>
          <el-table-column
            prop="contactPhone"
            label="联系电话"
            align="center"
          ></el-table-column>
          <el-table-column
            prop="customsChannelsIds"
            label="海关通道"
            align="center"
            :formatter="formatter"
          ></el-table-column>
          <el-table-column
            prop="createTime"
            label="创建时间"
            align="center"
          ></el-table-column>
          <el-table-column
            prop="creatorId"
            label="创建人"
            align="center"
          ></el-table-column>
          <el-table-column
            prop="status"
            label="审核状态"
            align="center"
            :formatter="forStatus"
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
                @click="handleEdit(scope.row)"
                v-if="scope.row.status == '1' || scope.row.status== '3'"
              >修改</el-button>
              <el-button
                type="success"
                size="small"
                @click="details(scope.row)"
              >详情</el-button>
              <el-button
                type="success"
                size="small"
                @click="handleChangde(scope.row)"
                v-if="scope.row.status == '1'"
              >变更口岸</el-button>
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
              :disabled="isDisabled"
              v-model="form.merchantName"
              placeholder="请输入企业名称"
            ></el-input>
          </el-form-item>
          <el-form-item
            label="企业税号"
            prop="merchantTaxCode"
          >
            <el-input
              :disabled="isDisabled"
              v-model="form.merchantTaxCode"
              placeholder="请输入企业税号"
            ></el-input>
          </el-form-item>
          <el-form-item
            label="法人姓名"
            prop="legalPersonName"
          >
            <el-input
              :disabled="isDisabled"
              v-model="form.legalPersonName"
              placeholder="请输入法人姓名"
            ></el-input>
          </el-form-item>
          <el-form-item
            label="法人身份证"
            prop="legalPersonIdCard"
          >
            <el-input
              :disabled="isDisabled"
              v-model="form.legalPersonIdCard"
              placeholder="请输入法人身份证"
            ></el-input>
          </el-form-item>
          <el-form-item
            label="工商注册地址"
            prop="companyAddress"
          >
            <el-input
              :disabled="isDisabled"
              v-model="form.companyAddress"
              placeholder="请输入工商注册地址"
            ></el-input>
          </el-form-item>
          <el-form-item
            label="经营范围"
            prop="businessScope"
          >
            <el-input
              :disabled="isDisabled"
              v-model="form.businessScope"
              placeholder="例如：计算机软件、计算机信息系统软件开发"
            ></el-input>
          </el-form-item>
          <el-form-item
            label="企业经营类别"
            prop="businessType"
          >
            <el-checkbox-group
              v-model="form.businessType"
              :disabled="isDisabled"
            >
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
              :disabled="isDisabled"
              v-model="form.contactName"
              placeholder="请输入联系人"
            ></el-input>
          </el-form-item>
          <el-form-item
            label="联系电话"
            prop="contactPhone"
          >
            <el-input
              :disabled="isDisabled"
              v-model="form.contactPhone"
              placeholder="请输入联系电话"
            ></el-input>
          </el-form-item>
          <el-form-item
            label="联系地址"
            prop="contactAddress"
          >
            <el-input
              :disabled="isDisabled"
              v-model="form.contactAddress"
              placeholder="请输入联系地址"
            ></el-input>
          </el-form-item>
          <el-form-item
            label="联系人邮箱"
            prop="contactMail"
          >
            <el-input
              :disabled="isDisabled"
              v-model="form.contactMail"
              placeholder="请输入联系地址"
            ></el-input>
          </el-form-item>
          <el-form-item
            label="网站名称"
            prop="webSiteName"
          >
            <el-input
              :disabled="isDisabled"
              v-model="form.webSiteName"
              placeholder="请输入网站名称"
            ></el-input>
          </el-form-item>
          <el-form-item
            label="网址"
            prop="webSiteAddress"
          >
            <el-input
              :disabled="isDisabled"
              v-model="form.webSiteAddress"
              placeholder="请输入网址"
            ></el-input>
          </el-form-item>
          <el-form-item
            label="经营商品"
            prop="businessGoods"
          >
            <el-input
              :disabled="isDisabled"
              v-model="form.businessGoods"
              placeholder="例如：服装、鞋帽、化妆品、母婴产品"
            ></el-input>
          </el-form-item>
          <el-form-item
            label="海关通道"
            prop="customsChannelsIds"
          >
            <el-checkbox-group v-model="form.customsChannelsIds">
              <el-checkbox label="1">深圳海关电子口岸</el-checkbox>
              <el-checkbox label="2">广州海关电子口岸</el-checkbox>
            </el-checkbox-group>
          </el-form-item>

          <el-form-item
            label="上传照片"
            prop="busLicensePic"
          >
            <el-upload
              class="upload-demo"
              ref="upload"
              :action="action()"
              :before-upload="beforeUpload"
              :on-preview="handlePreview"
              list-type="picture-card"
              :data="itemForm"
              :on-error="error"
              :on-success="successResave"
              :on-remove="handleRemove"
              :file-list="fileList"
              :show-file-list="true"
              :disabled="isDisabled"
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

                4、可上传法人身份证正反面、营业热照等 <br>

              </div>
            </el-upload>
            <el-dialog :visible.sync="dialogVisibleup">
              <img
                width="100%"
                :src="dialogImageUrl"
                alt=""
              >
            </el-dialog>
          </el-form-item>

          <el-form-item
            label="审核状态"
            prop="status"
            v-if="dltitle == '商家详情'"
          >
            <p>{{form.status}}</p>
          </el-form-item>
          <el-form-item
            label="审核时间"
            prop="reviewTime"
            v-if="dltitle == '商家详情'"
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
            v-if="dltitle == '商家详情'"
          >
            <el-input v-model="form.reviewRemark"></el-input>
          </el-form-item>

          <el-form-item>
            <el-button
              v-if="dltitle =='添加商家'"
              type="success"
              @click="addquarters('form')"
            >提交审核</el-button>
            <el-button
              v-if="dltitle=='修改商家'"
              type="success"
              @click="editquarters('form')"
            >确定修改</el-button>
            <el-button
              v-if="dltitle=='修改清关口岸'"
              type="success"
              @click="editquarters('form')"
            >提交</el-button>
            <!-- <el-button
              v-if=" dltitle=='添加商家'"
              @click="cancel('form')"
            >取消</el-button> -->
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
  // uploadFile,
} from '@/api/business'
import { uploadFile } from '@/api/uploadfile'
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
      isDisabled: false,
      formInline: {},
      dialogVisible: false,
      dialogVisibleup: false,
      dialogImageUrl: '',
      itemForm: {
        bucketName: 'customs',
        expires: '10000',
        perfixName: '2',
      },
      fd: '', //向服务器进行传递的参数（带有图片formdata）
      updateUrl: this.serverUrl + '/userInfo/update',
      bucketName: [],
      expires: 66666666,
      perfixName: '',
      uploadUrl: '',
      pageNum: 1,
      pageSize: 5,
      total: 0,
      value1: '',
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

      // uploadFile: null,
      // uploadDisabled: false,
      // // logoId: '1', //专区logo id
      // updialogVisible: false,
      fileList: [],
      // ruleForm: {
      //   dialogImageUrl: '1', //专区logo 上传到后台之后，后台会返回一个id,只需要给后台传id,但是点击编辑的时候后台返回的是http地址
      // },

      addOrEdit: 'add',
      form: {
        businessGoods: '', // 经营商品
        businessScope: '', // 业务范围
        businessType: [], // 业经营类型
        companyAddress: '', // 工商注册地址
        contactAddress: '', // 联系地址
        contactMail: '', // 联系邮箱
        contactName: '', // 联系人名称
        contactPhone: '', // 联系电话

        // createTime: '', // 创建时间
        // creatorId: '', // 创建人id
        // creatorName: '', // 创建人名称

        customsChannelsIds: [], // 海关通道 关联customs_channels中的id

        // id: '', // ID

        legalPersonIdCard: '', // 法人身份证
        legalPersonName: '', // 法人姓名

        merchantCode: 'A5S21X2Y1Y2Y3Y4', // 商户编码 格式字母加数字

        merchantName: '', // 企业名称
        merchantTaxCode: '54545645624644565', // 企业税号

        // pics: '', // 图片集合
        // reviewRemark: '', // 审核备注
        // reviewTime: '', // 审核时间
        // reviewerId: '', // 审核编号
        // reviewerName: '', // 审核人名称
        // status: '', // 商家状态
        // updateTime: '', // 修改时间

        webSiteAddress: '', // 网站地址
        webSiteName: '', // 网站名称
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
        ],
        customsChannelsIds: [
          {
            // 海关通道
            required: true,
            message: '必填',
            trigger: 'change',
          },
        ],
      },
    }
  },
  created() {
    this.getListMerchant()
    this.getChannelList()
  },
  methods: {
    getListMerchant() {
      getList({
        merchantName: this.formInline.merchantName,
        legalPersonName: this.formInline.legalPersonName,
        reviewerName: this.formInline.reviewerName,
        createTimeStar: this.formInline.createTimeStar,
        createTimeEnd: this.formInline.createTimeEnd,
        customsChannelsIds: this.formInline.customsChannelsIds,
        reviewTimeStar: this.formInline.reviewTimeStar,
        reviewTimeEnd: this.formInline.reviewTimeEnd,
        status: this.formInline.status,
        pageNum: this.pageNum,
        pageSize: this.pageSize,
      })
        .then((response) => {
          if (response.code == '200') {
            // Merchant list
            console.log(response, '返回的商家信息参数')
            this.merchantList = response.data.dataList
            this.statusList = response.data.dataList[0].status
            console.log(this.statusList, '状态信息6666666666')
            // console.log(555666)
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
    add() {
      // this.form = {
      //   businessGoods: '服装、鞋帽、化妆品、母婴产品、日用百货等商品',
      //   businessScope:
      //     '电子产品、计算机软硬件、绘图设备、服装机械的技术开发与销售， 服务半的设计和销,售，国内贸易（法律，行政法规，国务院决定规定在登记前须经批准项目除外），信息服务业务。',
      //   // businessType: ['1'],
      //   businessType: [],
      //   companyAddress: '广东深圳福田区卓越时代广场48楼',
      //   contactAddress: '广东深圳福田区卓越时代广场48楼',
      //   contactMail: '159893627887@139.com',
      //   contactName: '张三',
      //   contactPhone: '15989367887',
      //   // customsChannelsIds: 2,
      //   customsChannelsIds: [],
      //   legalPersonIdCard: '441424199202223760',
      //   legalPersonName: '张三',
      //   merchantName: '深圳明日网络科技有限公司',
      //   merchantTaxCode: '34250400750498127F',
      //   pics: '1212',
      //   merchantCode: 'Z6X1X1Y1Y2Y2Y4', // 商户编码
      //   webSiteAddress: 'https://www.xmfstore.com/admin-page#/mall/order',
      //   webSiteName: '中国品牌电商',
      // }
        this.form = {
        businessGoods: '', // 经营商品
        businessScope: '', // 业务范围
        businessType: [], // 业经营类型
        companyAddress: '', // 工商注册地址
        contactAddress: '', // 联系地址
        contactMail: '', // 联系邮箱
        contactName: '', // 联系人名称
        contactPhone: '', // 联系电话

        // createTime: '', // 创建时间
        // creatorId: '', // 创建人id
        // creatorName: '', // 创建人名称

        customsChannelsIds: [], // 海关通道 关联customs_channels中的id

        // id: '', // ID

        legalPersonIdCard: '', // 法人身份证
        legalPersonName: '', // 法人姓名

        merchantCode: 'Z5X1X3Y1Y2Y7Y4', // 商户编码 格式字母加数字

        merchantName: '', // 企业名称
        merchantTaxCode: '', // 企业税号

        pics: '123456', // 图片集合
        // reviewRemark: '', // 审核备注
        // reviewTime: '', // 审核时间
        // reviewerId: '', // 审核编号
        // reviewerName: '', // 审核人名称
        // status: '', // 商家状态
        // updateTime: '', // 修改时间

        webSiteAddress: '', // 网站地址
        webSiteName: '', // 网站名称
      },
      this.dltitle = '添加商家'
      this.dialogVisible = true
      this.isDisabled = false
    },
    // 详情
    details(row) {
      this.dltitle = '商家详情'
      console.log(row, '666668888888888888888')
      const params = {
        id: row.id,
      }
      getDetailMer(params).then((response) => {
        this.form = response.data
        this.dialogVisible = true
        console.log(response, '详情666666666666666666666')
      })
      this.isDisabled = false
    },
    // 编辑
    handleEdit(row) {
      this.dltitle = '修改商家'
      this.form = row
      this.dialogVisible = true
      this.isDisabled = false
      this.getListMerchant()
    },

    action() {
      return 'http://192.168.3.175:9502/customs/upload/file'
    },
    submitUpload() {
      this.$refs.upload.submit()
    },
    // 预览
    handlePreview(file) {
      this.dialogImageUrl = file.url
      this.dialogVisible = true
      console.log(file)
    },
    // 图片上传
    fileUp() {
      // debugger
      uploadFile(this.fd).then((response) => {
        if (response) {
          this.$message({
            showClose: true,
            type: 'success',
            message: '设置成功',
          })
          // this.initPage()
        }
        console.log(response, '图片上传成功')
        // this.dialogImageUrl = file.url
        // console.log(this.dialogImageUrl)
      })
    },
    beforeUpload(file) {
      // debugger
      const isJPG =
        file.type == 'image/jpeg' ||
        file.type == 'image/png' ||
        file.type == 'image/gif'
      const isLt2M = file.size / 1024 / 1024 < 2
      if (!isJPG) {
        this.$message.warning('上传头像图片只能是 JPG/PNG/GIF 格式!')
        return isJPG
      }
      if (!isLt2M) {
        this.$message.warning('上传头像图片大小不能超过 2MB!')
        return isLt2M
      }
      this.multfileImg = file
      // return isJPG && isLt2M
      this.fd = new FormData()
      //   //传其他参数
      this.fd.append('bucketName', this.itemForm.bucketName)
      this.fd.append('expires', this.itemForm.expires)
      this.fd.append('perfixName', this.itemForm.perfixName)
      this.fd.append('upfile', file)
      this.fileUp()
    },

    successResave(res, file) {
      console.log(666666666666)
      console.log(res, '返回的参数11111111111111')
      this.itemForm.upfile = URL.createObjectURL(file.raw)
    },
    //失败钩子
    error(response, file, fileList) {
      console.log(response)
    },

    addquarters(formName) {
      console.log(
        this.form.customsChannelsIds,
        '6666666666666666666668888888888888'
      )

      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.form.businessType = JSON.stringify(this.form.businessType)
          this.form.customsChannelsIds = JSON.stringify(
            this.form.customsChannelsIds
          )
          addMer(this.form)
            .then((response) => {
              console.log(response, '这是提交的参数信息')
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
    editquarters(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          console.log(this.form)
          editMer(this.form)
            .then((response) => {
              console.log(response)
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
    // 海关下拉表
    getChannelList() {
      channelList().then((response) => {
        this.customsChanneList = response.data
      })
    },

    // 变更口岸
    handleChangde(row) {
      console.log(row, '这是变更口岸的参数信息')
      this.dltitle = '修改清关口岸'
      if ((this.dltitle = '修改清关口岸')) {
        debugger
        this.isDisabled = true
        this.form = row
        this.dialogVisible = true
      }
    },

    // 图片
    handleRemove(file, fileList) {
      console.log(file, fileList)
      // this.uploadDisabled = false
    },
    handleSizeChange(val) {
      this.pageSize = val
      this.getListMerchant()
    },
    handleCurrentChange(val) {
      this.pageNum = val
      this.getListMerchant()
    },
    
    formatter(row, column) {
    //  return row.customsChannelsIds === ["1"] ? '深圳海关电子口岸' : (row.customsChannelsIds === ["2"] ? '广州海关电子口岸':'深圳海关电子口岸' + '广州海关电子口岸')
    //   }
       return row.customsChannelsIds === '1' ? '深圳海关电子口岸' : '广州海关电子口岸'
      },

      forStatus(row) {
        return row.status === 0 ? '待审核':(row.status === 1 ? '退回':(row.status === 2 ? '审核通过':(row.status === -1 ? '审核不通过':'')))
      }
  },
}
</script>

<style lang='scss' scoped>
.container {
  padding: 20px;
}
.container_on {
  // padding: 20px;
  height: 170px;
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
      width: 100px;
      & > span {
        color: #606266;
        font-weight: bold;
        font-size: 14px;
      }
    }
  }
  .two_nums {
    & > div {
      display: inline-block;
    }
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

