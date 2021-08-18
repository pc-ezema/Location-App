<template>
  <q-page class="q-pa-md bg-white">
        <q-resize-observer @resize="onResize"/>
        <transition appear enter-active-class="animated bounceInRight">
            <q-card class="no-border no-border">
            <q-toolbar class="text-black bg-grey-2 ">
                <q-item class="q-subtitle-1 q-pl-md">
                  <q-item-section class="text-h5">
                      <q-item-label lines="1">New Location</q-item-label>
                      <q-item-label caption lines="2">
                      <span class="text-weight-bold">Enter location details here</span>
                      </q-item-label>
                  </q-item-section>
                </q-item>

                <q-space/>

                <q-btn round flat icon="keyboard_backspace" @click="$router.push('/')"/>
                </q-toolbar>

                <q-separator></q-separator>

                <add-location-form></add-location-form>

            </q-card>
        </transition>
  </q-page>
</template>

<script>
import AddLocationForm from 'src/components/AddLocationForm.vue'

export default {
  name: "AddLocation",
  components: {
    'add-location-form': AddLocationForm
  },
  data() {
    return {
      tab: 'all',
      size: {},
    }
  },
  methods: {
    onResize(size) {
      this.size = size
    }
  }
}
</script>
