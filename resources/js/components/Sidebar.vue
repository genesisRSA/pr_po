<template>
    <v-navigation-drawer
      v-model="drawer"
      app
      color="yellow"
    >
      <v-sheet
        color="yellow"
        style="height:150px;"
      >
      <div class='d-flex'>
<v-toolbar-title class='toolbar_title'><img :src="require('../../../public/images/resbanner.png').default" style="width:270px; height:205px; position: relative; bottom: 55px; right:10px;"></v-toolbar-title>
<v-toolbar-title class='toolbar_title'></v-toolbar-title>
</div>
        <div class="mb-40" style="position:relative; bottom:100px; color: white;  font-weight: bold; left:20px;">Hello Genesis!</div>
      </v-sheet>
                                  <v-dialog transition="fab-transition"
                                v-model="logout_dialog"
                                max-width="290"
                                >
                                <v-card color="blue" dark>
                                    <v-card-title class="text-h5">
                                    Are you sure you want to logout?
                                    </v-card-title>

                                    <v-card-text>
                                    </v-card-text>

                                    <v-card-actions>
                                    <v-spacer></v-spacer>
                                    <v-btn
                                        color="white"
                                        text
                                        @click="logout()"
                                    >
                                        Yes
                                    </v-btn>

                                    <v-btn
                                        color="white"
                                        text
                                        @click="logout_dialog = false"
                                    >
                                        No
                                    </v-btn>
                                    </v-card-actions>
                                </v-card>
                            </v-dialog>

      <v-divider></v-divider>
      <v-list>
            <v-list-item>
                <form class="d-inline-flex align-center">
                    <v-hover v-slot="{ hover }">
                        <v-btn text type="submit" class="sideBarButton_2" :href="route('dashboard')" color="gray" x-large :style="{ 'background-color': hover ? 'gold' : '' }">
                            <v-icon color="gray" left small>mdi-view-dashboard</v-icon>
                            Dashboard
                        </v-btn>
                    </v-hover>
                </form>
            </v-list-item>
            <v-divider></v-divider>
            <!-- /////////////////////////////////////////////////////////////////// -->
            <div v-if="canViewRFQS == 'true'">
            <v-list-item>
                <form class="d-inline-flex align-center">
                    <v-hover v-slot="{ hover }">
                        <v-btn text type="submit" class="sideBarButton_3" :href="is_Admin==true ? route('req_for_quotation') : route('req_for_quotation_reg_user')" color="gray" x-large :style="{ 'background-color': hover ? 'gold' : '' }">
                            <v-icon color="gray" left small>mdi-comment-quote</v-icon>
                            RFQs
                        </v-btn>
                    </v-hover>
                </form>
            </v-list-item>
            <v-divider></v-divider>
            </div>
            <!-- /////////////////////////////////////////////////////////////////// -->
            <div v-if="canViewPR == 'true'">
            <v-list-item>
                <form class="d-inline-flex align-center">
                    <v-hover v-slot="{ hover }">
                        <v-btn text type="submit" class="purchase_req" :href="is_Admin==true ? route('purch_req') : route('purch_req_reg_user')" color="gray" x-large :style="{ 'background-color': hover ? 'gold' : '' }">
                            <v-icon color="gray" left small>mdi-account-arrow-right</v-icon>
                            Purchase Request
                        </v-btn>
                    </v-hover>
                </form>
            </v-list-item>
            <v-divider></v-divider>
            </div>
            <!-- /////////////////////////////////////////////////////////////////// -->
            <div v-if="canViewPO == 'true'">
            <v-list-item>
                <form class="d-inline-flex align-center">
                    <v-hover v-slot="{ hover }">
                        <v-btn text type="submit" class="purchase_order" :href="is_Admin==true ? route('purch_order') : route('purch_order_reg_user')" color="gray" x-large :style="{ 'background-color': hover ? 'gold' : '' }">
                            <v-icon color="gray" left small>mdi-order-bool-ascending</v-icon>
                            Purchase Order
                        </v-btn>
                    </v-hover>
                </form>
            </v-list-item>
            <v-divider></v-divider>
            </div>
            <!-- /////////////////////////////////////////////////////////////////// -->
            <div v-if="canViewDM == 'true'">
            <v-list-item>
                <form class="d-inline-flex align-center">
                    <v-hover v-slot="{ hover }">
                        <v-btn text type="submit" class="data_mngt" :href="is_Admin==true ? route('data_management') : route('data_management_reg_user')" color="gray" x-large :style="{ 'background-color': hover ? 'gold' : '' }">
                            <v-icon color="gray" left small>mdi-file-document</v-icon>
                            Data Management
                        </v-btn>
                    </v-hover>
                </form>
            </v-list-item>
            <v-divider></v-divider> 
            </div>
            <!-- /////////////////////////////////////////////////////////////////// -->
            <div v-if="is_Admin==true">
                    <v-list-item>
                        <form class="d-inline-flex align-center">
                            <v-hover v-slot="{ hover }">
                                <v-btn text type="submit" class="admin_autho" :href="route('admin_authorization')" color="gray" x-large :style="{ 'background-color': hover ? 'gold' : '' }">
                                    <v-icon color="gray" left small>mdi-cog</v-icon>
                                     Authorizations
                                </v-btn>
                            </v-hover>
                        </form>
                    </v-list-item>
                <v-divider></v-divider>
            </div>
            <v-list-item>
                <form class="d-inline-flex align-center" @click.prevent="logout_dialog=!logout_dialog">
                    <v-hover v-slot="{ hover }">
                        <v-btn text type="submit" class="sideBarButton" color="gray" x-large :style="{ 'background-color': hover ? 'gold' : '' }">
                            <v-icon color="gray" left small>mdi-logout</v-icon>
                            Logout
                        </v-btn>
                    </v-hover>
                </form>
            </v-list-item>
      </v-list>
    </v-navigation-drawer>
</template>
<script>
  export default {
    props:[
        'drawer'
    ],
    data: () => ({
      logout_dialog: false,
      links: [
        ['mdi-inbox-arrow-down', 'Inbox'],
        ['mdi-send', 'Send'],
        ['mdi-delete', 'Trash'],
        ['mdi-alert-octagon', 'Spam'],
        ['mdi-logout', 'Logout'],
      ],
      is_Admin : null,
      canViewRFQS : '',
      canViewPR : '',
      canViewPO : '',
      canViewDM : '',
    }),

    created: function(){
        this.is_admin()
        this.canView()
    },

    methods: {
        logout() {
            this.logout_dialog  = false;
            this.$inertia.post(route('logout'));
        },
        is_admin(){
            let url = window.location.href
            let url_result = url.includes('admin')
            if(url_result == true){
                this.is_Admin = true;
            }
        },
        canView() {
               axios.get('/canViewSideBar')
              .then(response =>{
                    this.canViewRFQS = response.data[0]
                    this.canViewPR = response.data[1]
                    this.canViewPO = response.data[2]
                    this.canViewDM = response.data[3]
              })
              .catch(error =>{
                    console.log(error.response);
              })
              .finally(() => {
                  
              });
        }
    },
  }
</script>
<style>
    .sideBarButton{
        width: 300%;
        background-color: white;
        right: 153px;
    }
    .sideBarButton_2{
        width: 300%;
        background-color: white;
        right: 190px;

    }
    .sideBarButton_3{
        width: 350%;
        background-color: white;
        right: 157px;
    }
    .purchase_req{
        width: 300%;
        background-color: white;
        right: 260px;
    }
    .purchase_order{
        width: 150%;
        background-color: white;
        right: 70px;
    }
    .admin_autho{
        width: 300%;
        background-color: white;
        right: 236px;
    }
    .data_mngt{
        width: 300%;
        background-color: white;
        right: 259px;
    }
</style>

