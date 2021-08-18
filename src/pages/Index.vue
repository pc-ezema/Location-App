<template>
  <q-page class="q-pa-md bg-white flex flex-center">
        <q-resize-observer @resize="onResize"/>
          <q-card-section class="text-h4 text-weight-bold">
            <div>Add Location</div>
          <div class="row justify-center">
            <q-btn round flat icon="add_box" color="green" style="font-size: 1em!important" @click="$router.push('/AddLocation')"> </q-btn>
          </div>
          </q-card-section>
  </q-page>
</template>

<script>

export default {
  name: "Index",
  data() {
    return {
      size: {}
    }
  },
  methods: {
    onResize(size) {
      this.size = size
    }
  }
}
</script>
