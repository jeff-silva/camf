<template>
  <div>
    <v-btn icon @click="shown=!shown" ref="btn">
      <slot name="activator">
        Button
      </slot>
    </v-btn>

    <v-navigation-drawer
      v-if="type=='drawer'"
      v-model="shown"
      absolute
      temporary
      right
      app
      hide-overlay
      height="100vh"
      elevation="0"
    >
      <div v-if="title" class="pa-3" style="background:#00000044;">
        {{ title }}
      </div>
      <slot></slot>
    </v-navigation-drawer>

    <template v-if="type=='menu'">
      <div ref="menu" style="position:relative;"></div>
      <v-menu
        v-model="shown"
        :attach="$refs.menu"
        max-width="250px"
        left
        :position-x="50"
        :position-y="15"
      >
        <v-sheet class="pa-2" style="width:250px;">
          <slot></slot>
        </v-sheet>
      </v-menu>
    </template>
  </div>
</template>

<script>
export default {
  props: {
    type: {
      type: String,
      default: 'menu', // drawer, menu
    },
    title: {
      type: [Boolean, String],
      default: false,
    },
  },
  data() {
    return {
      shown: false,
    };
  },
};
</script>