<template>
  <div>
    <v-list v-if="type=='vertical'" color="transparent">
      <template v-for="(item, i) in compItems">
        <template v-if="item=='divider'">
          <v-divider :key="i" />
        </template>
        <template v-else>
          <v-list-item :key="i" :to="item.to" @click="item.click" router exact>
            <v-list-item-action v-if="item.icon">
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title v-text="item.title" />
            </v-list-item-content>
          </v-list-item>
        </template>
      </template>
    </v-list>

    <div v-if="type=='horizontal'" class="d-flex">
      <template v-for="(item, i) in compItems">
          <template v-if="item=='divider'">
            <v-divider :key="i" vertical />
          </template>

          <template v-else-if="item.children.length==0">
            <v-btn :key="i" text>
              {{ item.title }}
            </v-btn>
          </template>

          <template v-else>
            <v-menu :key="i" offset-y open-on-hover>
              <template #activator="{ on, attrs }">
                <v-btn
                  :key="i"
                  text
                  v-bind="attrs"
                  v-on="on"
                >
                  {{ item.title }}
                  <v-icon class="ms-1" size="15px">mdi-chevron-down</v-icon>
                </v-btn>
              </template>

              <v-card class="pa-0" style="width:200px;">
                <app-nav type="vertical" :items="item.children"></app-nav>
              </v-card>
            </v-menu>
          </template>
      </template>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    type: {
      type: String,
      default: 'vertical',
    },
    items: {
      type: Array,
      default: () => ([]),
    },
  },
  computed: {
    compItems() {
      return this.items.map(item => {
        if (typeof item != 'object') return item;
        return {
          to: null,
          title: '',
          icon: false,
          click: () => {},
          children: [],
          ...item
        };
      });
    },
  },
};
</script>