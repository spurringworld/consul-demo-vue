<template>
  <div class="home">
    <el-row :gutter="24" style="padding-top: 5px;">
      <el-col :span="4"><br></el-col>
      <el-col :span="16">
        <el-card class="box-card">
          <el-tabs v-model="activeName">

            <el-tab-pane label="账户查询" name="aa">
              <div style="text-align: left;">
                <el-form ref="form1" label-width="111px">
                  <el-form-item label="场景描述:">
                    <span>访问 account-service 服务，获取账户信息。</span>
                  </el-form-item>
                  <el-form-item label="账户ID:">
                    <el-input-number v-model="aid" :min="1" :max="10"></el-input-number>
                  </el-form-item>
                  <el-form-item>
                    <el-button type="primary" @click="getAccount" plain>查询</el-button>
                  </el-form-item>
                </el-form>
                <el-divider></el-divider>
                <el-steps :space="200" :active="2">
                  <el-step title="gateway-service"></el-step>
                  <el-step title="account-service"></el-step>
                </el-steps>
              </div>
              <div>
                <el-input 
                  v-model.trim="accountData"
                  type="textarea"
                  :rows="11" />
              </div>
            </el-tab-pane>

            <el-tab-pane label="产品查询" name="bb">
              <div style="text-align: left;">
                <el-form ref="form1" label-width="111px">
                  <el-form-item label="场景描述:">
                    <span>访问 product-service 服务，获取产品信息。</span>
                  </el-form-item>
                  <el-form-item label="产品ID:">
                    <el-input-number v-model="pid" :min="1" :max="10"></el-input-number>
                  </el-form-item>
                  <el-form-item>
                    <el-button type="primary" @click="getProduct" plain>查询</el-button>
                  </el-form-item>
                </el-form>
                <el-divider></el-divider>
                <el-steps :space="200" :active="2">
                  <el-step title="gateway-service"></el-step>
                  <el-step title="product-service"></el-step>
                </el-steps>
              </div>
              <div>
                <el-input 
                  v-model.trim="productData"
                  type="textarea"
                  :rows="11" />
              </div>
            </el-tab-pane>

            <el-tab-pane label="消费者账号查询" name="cc">
              <div style="text-align: left;">
                <el-form ref="form1" label-width="111px">
                  <el-form-item label="场景描述:">
                    <span>访问 customer-service 服务获取消费者信息，customer-service 服务在内部调用 account-service 服务，获取消费者的账户信息。</span>
                  </el-form-item>
                  <el-form-item label="消费者ID:">
                    <el-input-number v-model="cid" :min="1" :max="3"></el-input-number>
                  </el-form-item>
                  <el-form-item>
                    <el-button type="primary" @click="getCustomerWithAccount" plain>查询</el-button>
                  </el-form-item>
                </el-form>
                <el-divider></el-divider>
                <el-steps :space="200" :active="3">
                  <el-step title="gateway-service"></el-step>
                  <el-step title="customer-service"></el-step>
                  <el-step title="account-service"></el-step>
                </el-steps>
              </div>
              <div>
                <el-input 
                  v-model.trim="customerAccountData"
                  type="textarea"
                  :rows="22" />
              </div>
            </el-tab-pane>

            <el-tab-pane label="创建订单" name="dd">
              <div style="text-align: left;">
                <el-form ref="form1" label-width="111px">
                  <el-form-item label="场景描述:">
                    <span>访问 order-service 服务生成新的订单，order-service 服务在内部调用 customer-service 和 product-service 服务，获取消费者和产品的详细信息，customer-service 服务又会调用 account-service 服务，获取消费者的账户信息。</span>
                  </el-form-item>
                  <el-form-item label="消费者:">
                    <el-select v-model="ocid" placeholder="请选择">
                      <el-option
                        v-for="item in cList"
                        :key="item.id"
                        :label="item.name"
                        :value="item.id">
                      </el-option>
                    </el-select>
                  </el-form-item>
                  <el-form-item label="产品:">
                    <el-select v-model="opids" multiple placeholder="请选择">
                      <el-option
                        v-for="item in pList"
                        :key="item.id"
                        :label="item.name + ' -- ￥' + item.price"
                        :value="item.id">
                      </el-option>
                    </el-select>
                  </el-form-item>
                  <el-form-item>
                    <el-button type="primary" @click="postOrder" plain>下订单</el-button>
                  </el-form-item>
                </el-form>
                <el-divider></el-divider>
                <el-steps :space="200" :active="4">
                  <el-step title="gateway-service"></el-step>
                  <el-step title="order-service"></el-step>
                  <el-step title="customer-service"></el-step>
                  <el-step title="account-service"></el-step>
                </el-steps>
                <el-steps :space="200" :active="3">
                  <el-step title="gateway-service"></el-step>
                  <el-step title="order-service"></el-step>
                  <el-step title="product-service"></el-step>
                </el-steps>
              </div>
              <div>
                <el-input 
                  v-model.trim="orderInitData"
                  type="textarea"
                  :rows="16" />
              </div>
            </el-tab-pane>

            <el-tab-pane label="完成订单" name="ee">
              <div style="text-align: left;">
                <el-form ref="form1" label-width="111px">
                  <el-form-item label="场景描述:">
                    <span>访问 order-service 服务确认完成订单，order-service 服务在内部调用 account-service 服务完成扣款操作。</span>
                  </el-form-item>
                  <el-form-item label="订单ID:">
                    <el-input-number v-model="oid" :min="1" :max="10000"></el-input-number>
                  </el-form-item>
                  <el-form-item>
                    <el-button type="primary" @click="putOrder" plain>完成</el-button>
                  </el-form-item>
                </el-form>
                <el-divider></el-divider>
                <el-steps :space="200" :active="3">
                  <el-step title="gateway-service"></el-step>
                  <el-step title="order-service"></el-step>
                  <el-step title="account-service"></el-step>
                </el-steps>
              </div>
              <div>
                <el-input 
                  v-model.trim="orderDoneData"
                  type="textarea"
                  :rows="16" />
              </div>
            </el-tab-pane>

          </el-tabs>
          
        </el-card>
      </el-col>
      <el-col :span="4"><br></el-col>
    </el-row>

  </div>

</template>
<script>
  import axios from 'axios'
  export default {
    data() {
      return {
        aid: 1,
        cid: 1,
        pid: 1,
        oid: 1,
        activeName: 'aa',
        accountData: "",
        productData: "",
        customerAccountData: "",
        orderInitData: "",
        orderDoneData: "",
        pList: [],
        cList: [],
        ocid: 1,
        opids: []
      };
    },
    created(){
      this.initOrderPage();
      document.title = 'flomesh-consul-demo';
    },
    methods: {
      getAccount() {
        axios.get('/api/account/' + this.aid).then(response =>{
          this.accountData = JSON.stringify(response.data, null, 4);
        }).catch((err)=>{
          this.$alert(err)
        })
      },
      getProduct() {
        axios.get('/api/product/' + this.pid).then(response =>{
          this.productData = JSON.stringify(response.data, null, 4);
        }).catch((err)=>{
          this.$alert(err)
        })
      },
      getCustomerWithAccount() {
        axios.get('/api/customer/withAccounts/' + this.cid).then(response =>{
          this.customerAccountData = JSON.stringify(response.data, null, 4);
        }).catch((err)=>{
          this.$alert(err)
        })
      },
      postOrder() {
        axios.post('/api/order/', {
          customerId: this.ocid,
          productIds: this.opids
        }).then(response =>{
          this.orderInitData = JSON.stringify(response.data, null, 4);
        }).catch((err)=>{
          this.$alert(err)
        })
      },
      putOrder() {
        axios.put('/api/order/' + this.oid).then(response =>{
          this.orderDoneData = JSON.stringify(response.data, null, 4);
        }).catch((err)=>{
          this.$alert(err)
        })
      },
      initOrderPage() {
        axios.post('/api/customer/ids/', [1,2,3]).then(response =>{
          this.cList = response.data;
          // console.log(JSON.stringify(this.cList))
        }).catch((err)=>{
          this.$alert("get customers " + err)
        });
        axios.post('/api/product/ids/', [1,2,3,4,5,6,7,8,9,10]).then(response =>{
          this.pList = response.data;
          // console.log(JSON.stringify(this.pList))
        }).catch((err)=>{
          this.$alert("get products " + err)
        })
      }
    }
  };
</script>