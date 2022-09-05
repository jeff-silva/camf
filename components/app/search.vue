<template>
  <div>
    <div class="d-flex">
      <div class="d-none d-lg-block me-4" :style="`min-width:${filterWidth}; max-width:${filterWidth};`">
        <slot name="filter"></slot>
        <v-btn block>Filtrar</v-btn>
      </div>
      <div class="flex-grow-1">
        <v-btn block class="mb-3 d-lg-none" @click="drawer=true">Filtro</v-btn>
        <slot name="results"></slot>
      </div>
    </div>

    <v-navigation-drawer
      v-model="drawer"
      fixed
      temporary
      right
      app
      hide-overlay
    >
      <div class="d-flex flex-column" style="height:100vh;">
        <div class="pa-3 flex-grow-1">
          <slot name="filter"></slot>
        </div>
        <v-divider />
        <div class="pa-3">
          <v-btn block>Filtrar</v-btn>
        </div>
      </div>
    </v-navigation-drawer>
  </div>
</template>

<script>
export default {
  props: {
    filterWidth: {
      type: String,
      default: '300px',
    },
  },
  data() {
    return {
      drawer: false,
    };
  },
};
</script>