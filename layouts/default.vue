<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
      color="#111"
    >
      <div class="d-flex flex-column" style="height:100vh;">

        <!-- Logged -->
        <v-sheet v-if="!miniVariant && user">
          <div class="pa-4 text-uppercase">{{ user.name }}</div>
        </v-sheet>

        <!-- Unlogged -->
        <v-sheet v-if="!user" class="pa-3">
          <v-btn block color="primary" @click="login(true)">Login</v-btn>
        </v-sheet>

        <v-divider />

        <div class="flex-grow-1" style="overflow:auto;">
          <app-nav :items="[
            {icon:'mdi-server', to:'/', title:'Play'},
          ]" />
        </div>

        <v-divider />
        <div class="pa-2">
          <div>cstrikeclub0@gmail.com</div>
          <div>Version: 0.1.4#819</div>
        </div>
      </div>
    </v-navigation-drawer>
    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
    >
      <v-app-bar-nav-icon @click.stop="drawer=!drawer" v-if="$vuetify.breakpoint.mobile" />
      <v-btn text small>
        <v-img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAMIAAAApCAMAAABQvAZCAAAB0VBMVEUAAAD/////////AJn/////////////////////////zAD////////////////////////////9VR//////////////////////////////////////////////////////////////////////////////////////////////////zAD/////AJn8Ux3/////////////////AJn8Ux3/mzH////////////////////8Ux3////////////////////9TSj/////////////////AJn/////AJn/AJn////////////////////////////////8Ux38Ux38Ux3////////8Ux38Ux3/mTP/////zAD/mzH/mTP/mTP////8Ux38Ux3/AJn/////zAD/nDD/mTP8Ux3/AJn/mTP9ZiP9ZSP9LVb////+IWf8Ux3/AJn8Ux39dCf+giz+J1//zAD/AJn/zAD/////AJn8Ux3/mTP/zAD/Bo/9NEv/DYX9QTf/lTH+hy3/oir9RjD+G3H9PED/ni7+gCv/qSP9aSP8WR//sxn/xAj+FXj+I2T+LFf+jS/+giz+fCr8Syn8TiT9XiD8UR//uRP/vBD+En62QXV+AAAAeHRSTlMARO8imeFV/Wb7ZvTWegMPCDwhIBXu58hfTuvObQYC0cCzqodoQAz22buheHRaWirkw6WWeXhRMycdEty3nZJiNxwa8ZSOeFgX1rCui4N+S0cYDAXg3tPSxDAu+PTv6tu3rHBaSTwmFOTk4uLZ2JaWiop9eFo/NzAMfNQFAAAFuElEQVRYw7XZd1/TQBjA8acoxErpoEs66K6CaHGVgizFvffee+9xqYqCe+/5ak2TJne555pEq9+/Slo+zS+5XJIGeD5XYnRDdF4kPEEIaQlHPIuym8bGkyBQdGE+0BVcAiVglFwi0BRvYbVHIiJz0x0V4PURbAvoWohAChhRIgJCt2djd4E31NFKrETXdgOrezfBIknLBGk7832S84RTB+9j88Es2ddC7HhMe7mLiKxlE7A5YFhLhBwW4IRKnDggjfjB0CPeV9YJi8Ew4jjh9KHnjx+/fPnm51uLhK3txJmoMRJKEhEasEyI0MHYT4REBfd0z1++eStO6HITpzwh0IwSsV6UICyEEnGYsP/wPZPHSgVKONBOnFvk1SbfRv8zN2SZ0MHNZ4i4gKv4wSX4PcQOXokEWh7LLG6l7zdKSEPdsLOE/UfuiTz/YUroIALt8Ux2Q0oSbeZBUMzjB1g5CRCs7ZpWr1WCuwgq/1xHCWf0AuTxT5rgw1/mXhzQ1iOUFxx0MwFgGzdV5WqfH9A+XGYSsC5QuUgD5oLzUx+nP734/OLT9McpPmIZ6PCQSJXAUMzg3VAEyJiTT4Ki3E7HSuOEVaDa5CSh88LrKvX64o3pKWHCIsKJ+4DhTQtOXxUJn7D6jGUllDCxh+jmgSpFF/Q3SlixvMra1bmwWn31eQolrJT4oyAIJkE0arOwWpvkU6mY+netdCYz0lBCS46+DnLfOtqKE8QFoCQoXk1zCXMIJwecXjT3FJX1a02sBMVgPuWuvRjoCoyXO3rc6kbwoYQAfZ3nvnVbg4Sze/kCJUHz7YUpYRXhBIGDL4Y2EzJsjLbuAlCDq2oRfSghSf/cCIrFdKcnUYKoYHkn0AQlYpomoEudPcDzzeLFSNgHDbjaCTnazSdAlr72KtkxosuAMGEHV7AC1ATq1ZSegCb4EbBVUI7BfNd40A+YNka2ogRm5AQABphxJUzYsQ8VaAnU65tQF0Y3LbY2GmuW2U63fjlQCnlB28LDKIE5fjebzqZBlCAswAnVpVDHX+Hlwc6g2xjWzHAaUkdkeF40o6y8VOETIM5ej6fpWR1wAlvw5dHk5KUVYJlAOLPATs6YDoHlzxBqFCVspifCoSLdbpsECTuPVVWPPrx7IMvy+nVQd+vZ93+yF/wx0+2Lj74RZda5yCcw02obM8cVcIJW8Oh9bfVNBbBAfvD+EU4IowtRS3RK96hHc6APDMEJYkjwCV76RSOrmCkVJew8rqzes1+yTAtoguLh5Hea8HczUpxOLADluQNA5Zkr1wkugTkV9O9mThJ8Qq3g2UMZF2gJWgSbgM8LcTxyXKwx9sI/4SZJoLrjRKxF23vYGEpYWv3yVDacuzODcUJf/O6JknC9U4XPztJ2PHJE8tqBHUEn8kYJIYlggyjh6uQD2YEPVR1eQXww9BCRAiR71UlRcaCnZyQENZcbJ0CUIClACVdkZ55+NRIG+Y0TC4FJgAh1FYfVEwEoZtVu1sa5O0CUsIUgOZywRHbo4RM9Ad8vDHuBEdpNhKL1ce/Xr2Xdq5X2oEXCuGBXOk/AHmjzq/CuLTPETJQeYi0A0B2pz5C9bfTjKEH5GFqcFCU4b1D3g/jeOdYRBFVpE75Lx2OhQOy0iG88smCbYN/QcPy2LtqwIR0j9vq9kHWYcBJNqU0myA+VY1r/HakJYxXJYcKQm5u/VzabID+tVvVf85oQGSaOEvDMEYemE+TJKv1NtUn2CXha3SxKWDejsRMy65q6rBPqDWg//JeEATSZ4QSwsEBmrQGTSor8hQp/MyRZJ4BpWg17/2kC+NFTHntR4KVtEnrx45LmE6iQzbO2o2haTwBvzCahDT+0ai7B+RNPdzwXgJn8whDwiu3WCT5m3pBCTSeIDW1LjGbTeyLh2spMhPuV5865WeN+UKyeadYHWGImZ21bTRnqtrUZthpPydpY8Kd+A3BjQEJFoWr/AAAAAElFTkSuQmCC" />
      </v-btn>

      <app-nav type="horizontal" :items="[
        {to:'', title:'Cams', children:[
          {to:'/females', title:'Females', children:[]},
          {to:'/males', title:'Males', children:[]},
          {to:'/trans', title:'Trans', children:[]},
          {to:'/couples', title:'Couples', children:[]},
          {to:'/all', title:'All', children:[]},
        ]},
        {to:'', title:'Private Shows', children:[]},
        {to:'', title:'Explore', children:[
          {to:'', title:'Trending Tags', children:[]},
          {to:'', title:'Tokens', children:[]},
          {to:'', title:'Gold CAM4', children:[]},
          {to:'', title:'Shop Swag', children:[]},
          {to:'', title:'CAM4 Awards', children:[]},
        ]},
        {to:'', title:'Broadcast', children:[]},
        {to:'', title:'Local Dates', children:[]},
        {to:'', title:'HD Cams', children:[]},
      ]" />

      <v-spacer />

      <app-btn-drawer
        v-if="user"
        type="menu"
        title="Notifications"
      >
        <template #activator>
          <v-icon>mdi-bell</v-icon>
        </template>

        <div class="pa-4">
          Notifications
        </div>
      </app-btn-drawer>

      <app-btn-drawer
        type="drawer"
        title="Invites"
      >
        <template #activator>
          <v-icon>mdi-email-variant</v-icon>
        </template>

        <div class="pa-4">
          Invites
        </div>
      </app-btn-drawer>

      <app-btn-drawer
        type="menu"
        title="Help"
      >
        <template #activator>
          <v-icon>mdi-help-circle-outline</v-icon>
        </template>

        <app-nav :items="[
          {to:'/faq', title:'FAQ'},
          {to:'/game-modes', title:'Game modes'},
          {to:'/credits', title:'Credits'},
          {to:'/about', title:'About'},
          {to:'/rules', title:'Rules'},
          {to:'/contacts', title:'Contacts'},
        ]" />
      </app-btn-drawer>

      <app-btn-drawer
        type="menu"
        title="Language"
      >
        <template #activator>
          <v-icon>mdi-flag</v-icon>
        </template>

        <app-nav :items="[
          {to:null, title:'English'},
          {to:null, title:'Português'},
        ]" />
      </app-btn-drawer>
      
      <app-btn-drawer
        type="menu"
        title="User"
      >
        <template #activator>
          <img :src="user.photo" style="width:40px; height:40px; border-radius:30px;" />
        </template>

        <app-nav :items="[
          {to:null, title:'My Profile'},
          {to:null, title:'My Account'},
          {to:null, title:'My Library'},
          {to:null, title:'My Calendar'},
          'divider',
          {to:null, title:'Broadcaster dashboard'},
          {to:null, title:'Broadcaster training'},
          'divider',
          {to:null, title:'FAQ'},
          {to:null, title:'Logout'},
        ]" />
      </app-btn-drawer>
    </v-app-bar>
    <v-main>
      <v-container fluid>
        <nuxt />
      </v-container>
    </v-main>
    <v-footer :absolute="!fixed">
      <v-spacer />
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data () {
    return {
      clipped: true,
      drawer: true,
      fixed: false,
      miniVariant: false,
      right: true,
      title: 'Play-CS.com',
      user: false,
    };
  },
  methods: {
    async login(modal=false) {
      const { data } = await this.$axios.get('https://randomuser.me/api/?results=1');

      const setUser = () => {
        this.user = {
          name: data.results[0].login.username,
          photo: data.results[0].picture.medium,
        };
      };

      if (modal) {
        let w = window.open("", "MsgWindow", "width=600,height=400");
        w.document.write(`<pre>Connecting to social authentication service</pre>`);

        setTimeout(() => {
          w.close();
          setUser();
        }, 2000);

        return;
      }

      setUser();
    },
    logout() {
      this.user = false;
    },
  },
  mounted() {
    this.login();
  },
}
</script>
