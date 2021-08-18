 <template>
  <q-layout view="hHh lpR fFf" class="bg-grey-1">
    <q-header elevated class="bg-white text-grey-8 q-py-xs" height-hint="58">
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          @click="leftDrawerOpen = !leftDrawerOpen"
          aria-label="Menu"
          icon="menu"
        />

        <q-btn flat no-caps no-wrap class="q-ml-xs" v-if="$q.screen.gt.xs">
          <q-toolbar-title shrink class="text-weight-bold">
            Location
          </q-toolbar-title>
        </q-btn>

        <q-space />

        <div class="YL__toolbar-input-container row no-wrap">
          <q-input dense outlined square v-model="search" placeholder="Search" class="bg-white col" />
          <q-btn class="YL__toolbar-input-btn" color="grey-3" text-color="grey-8" icon="search" unelevated />
        </div>

        <q-space />

        <div class="q-gutter-sm row items-center no-wrap">
          <q-btn round dense flat color="grey-8" icon="notifications">
            <q-badge color="red" text-color="white" floating>
              2
            </q-badge>
            <q-tooltip>Notifications</q-tooltip>
          </q-btn>
          <q-btn round flat>
            <q-avatar size="26px">
              <img src="https://cdn.quasar.dev/img/boy-avatar.png">
            </q-avatar>
            <q-tooltip>Account</q-tooltip>
          </q-btn>
        </div>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
      class="bg-grey-2"
      :width="240"
    >
      <q-scroll-area class="fit">
        <q-list padding>
          <q-item to=""> 
            <q-item-section avatar>
              <q-icon color="grey" name="home" />
            </q-item-section>
            <q-item-section>
              <q-item-label>General</q-item-label>
            </q-item-section>
          </q-item>
          <q-expansion-item>
            <template v-slot:header>
              <q-item-section avatar>
                <q-icon color="grey" name="location_city" />
              </q-item-section>

              <q-item-section>
                Location
              </q-item-section>
            </template>

            <q-list class="q-pl-lg">
              <q-item to="/" active-class="q-item-no-link-highlighting">
                <q-item-section avatar>
                  <q-icon color="grey" name="location_on"/>
                </q-item-section>
                <q-item-section>
                  <q-item-label>Add Location</q-item-label>
                </q-item-section>
              </q-item>
              <q-item to="/Location" active-class="q-item-no-link-highlighting">
                <q-item-section avatar>
                  <q-icon color="grey" name="my_location"/>
                </q-item-section>
                <q-item-section>
                  <q-item-label>Locations</q-item-label>
                </q-item-section>
              </q-item>
            </q-list>
  
          </q-expansion-item>
          <q-item to=""> 
            <q-item-section avatar>
              <q-icon color="grey" name="menu" />
            </q-item-section>
            <q-item-section>
              <q-item-label>Menu/Items</q-item-label>
            </q-item-section>
          </q-item>
          <q-item to=""> 
            <q-item-section avatar>
              <q-icon color="grey" name="list" />
            </q-item-section>
            <q-item-section>
              <q-item-label>Tax</q-item-label>
            </q-item-section>
          </q-item>

          <q-separator class="q-my-md" />

        </q-list>
      </q-scroll-area>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
export default {
  name: 'MyLayout',
  data () {
    return {
      leftDrawerOpen: false,
      search: '',
    }
  }
}
</script>

<style lang="sass">
.YL
  &__toolbar-input-container
    min-width: 100px
    width: 55%
  &__toolbar-input-btn
    border-radius: 0
    border-style: solid
    border-width: 1px 1px 1px 0
    border-color: rgba(0,0,0,.24)
    max-width: 60px
    width: 100%
  &__drawer-footer-link
    color: inherit
    text-decoration: none
    font-weight: 500
    font-size: .75rem
    &:hover
      color: #000
</style>
