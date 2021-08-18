<template>
    <div>
        <q-card-section class="q-pa-sm">
            <q-list class="row">
            <q-item class="col-lg-4 col-md-4 col-sm-4 col-xs-4 hidden">
                <q-item-section>
                <q-input color="black" v-model="add_location.p_loc_id" dense />
                </q-item-section>
            </q-item>
            <q-item class="col-lg-4 col-md-4 col-sm-4 col-xs-4 hidden">
                <q-item-section>
                <q-input color="black" v-model="add_location.user_id" dense/>
                </q-item-section>
            </q-item>
            <q-item class="col-lg-4 col-md-4 col-sm-4 col-xs-4 hidden">
                <q-item-section>
                <q-input color="black" v-model="add_location.page_id" dense/>
                </q-item-section>
            </q-item>
            <q-item class="q-subtitle-1 q-pl-md q-pt-lg">
                <q-item-section class="text-h6">
                    <q-item-label lines="1">Address</q-item-label>
                </q-item-section>
            </q-item>
            <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                <q-item-section>
                <q-input color="black" v-model="add_location.addr_1" dense label="Address Line 1 *"/>
                <span class="text-red" v-if="$v.add_location.addr_1.$error">Field required</span>
                </q-item-section>
            </q-item>
            <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                <q-item-section>
                <q-input color="black" v-model="add_location.addr_2" dense label="Address Line 2"/>
                </q-item-section>
            </q-item>
            <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
                <q-item-section>
                <q-input color="black" v-model="add_location.city" dense label="City *"/>
                <span class="text-red" v-if="$v.add_location.city.$error">Field required</span>
                </q-item-section>
            </q-item>
            <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
                <q-item-section>
                <q-input color="black" v-model="add_location.state" dense label="State *" />
                <span class="text-red" v-if="$v.add_location.state.$error">Field required</span>
                </q-item-section>
            </q-item>
            <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                <q-item-section>
                <q-input color="black" v-model="add_location.postcode" dense label="Postal Code *"/>
                <span class="text-red" v-if="$v.add_location.postcode.$error">Must be a numeric value </span>
                </q-item-section>
            </q-item>

            <q-item class="q-subtitle-1 q-pl-md q-pt-lg">
                <q-item-section class="text-h6">
                    <q-item-label lines="1">Contact Details</q-item-label>
                </q-item-section>
            </q-item>
            <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                <q-item-section>
                <q-input color="black" v-model="add_location.website" dense label="Website *"/>
                <span class="text-red" v-if="$v.add_location.website.$error"> URL address required </span>
                </q-item-section>
            </q-item>
            <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
                <q-item-section>
                <q-input color="black" v-model="add_location.email" dense label="Email *"/>
                <span class="text-red" v-if="$v.add_location.email.$error">Email address required </span>
                </q-item-section>
            </q-item>
            <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
                <q-item-section>
                <q-input color="black" v-model="add_location.tele" dense label="Telephone *"/>
                <span class="text-red" v-if="$v.add_location.tele.$error">Must be a numeric value </span>
                </q-item-section>
            </q-item>
            
            <q-item class="q-subtitle-1 q-pt-lg q-pt-lg hidden">
                <q-item-section class="text-h6">
                    <q-item-label lines="1">Geolocation</q-item-label>
                </q-item-section>
            </q-item>
            <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12 hidden">
                <q-item-section>
                <q-input color="black" v-model="add_location.latitude" dense label="Laititude *" autofocus/>
                </q-item-section>
            </q-item>
            <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12 hidden">
                <q-item-section>
                <q-input color="black" v-model="add_location.longitude" dense label="Longtitude *" autofocus/>
                </q-item-section>
            </q-item>
            <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12 hidden">
                <q-item-section>
                <q-input color="black" v-model="add_location.timezone" dense label="Time Zone *" autofocus/>
                </q-item-section>
            </q-item>

            <q-item class="q-subtitle-1 q-pt-lg q-pt-lg">
                <q-item-section class="text-h6">
                    <q-item-label lines="1">Others</q-item-label>
                </q-item-section>
            </q-item>
            <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                <q-item-section>
                <q-input color="black" v-model="add_location.distOffset" dense label="Distance Type *"/>
                <span class="text-red" v-if="$v.add_location.distOffset.$error">Numeric value required </span>
                </q-item-section>
            </q-item>
            <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                <q-item-section>
                <q-input color="black" v-model="add_location.telephone_display" dense label="Telephone Display *"/>
                <span class="text-red" v-if="$v.add_location.telephone_display.$error">Field required </span>
                
                </q-item-section>
            </q-item>
            <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                <q-item-section>
                <q-input color="black" v-model="add_location.distance_type" dense label="Distance Offset *"/>
                <span class="text-red" v-if="$v.add_location.distance_type.$error"> Field required </span>
                </q-item-section>
            </q-item>
            <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                <q-item-section>
                <q-input color="black" v-model="add_location.seq" dense label="Sequence *"/>
                <span class="text-red" v-if="$v.add_location.seq.$error">Numeric value required</span>
                </q-item-section>
            </q-item>
            <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                <q-item-section>
                <q-input color="black" v-model="add_location.loc_page_seq" dense label="Location Page Sequence *"/>
                <span class="text-red" v-if="$v.add_location.loc_page_seq.$error">Numeric value required </span>
                </q-item-section>
            </q-item>
            <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                <q-item-section>
                <q-input color="black" v-model="add_location.comments" dense label="Comments *"/>
                <span class="text-red" v-if="$v.add_location.comments.$error"> Field required </span>
                </q-item-section>
            </q-item>
            </q-list>
        </q-card-section>
        <q-card-actions align="right">
            <q-btn class="text-capitalize bg-positive text-white" @click="addLocation()">Submit</q-btn>
            <q-btn class="text-capitalize bg-negative text-white" @click="$router.push('/')">Cancel</q-btn>
        </q-card-actions>
    </div>
</template>

<script>
import { Notify } from 'quasar'
import { required, email, numeric, url } from 'vuelidate/lib/validators'

export default {
  name: "AddLocationForm",
  data: () => ({
    add_location: {
        p_loc_id: 70,
        user_id: 2468,
        addr_1: "",
        addr_2: "",
        city: "",
        state: "",
        postcode: "",
        tele: "",
        email: "",
        website: "",
        distance_type: "",
        seq: "",
        loc_page_seq: "",
        telephone_display: "",
        distOffset: "",
        comments: "",
        page_id: 1234
    },
  }),
  validations: {
      add_location: {
        p_loc_id: { required },
        user_id: { required },
        addr_1: { required },
        city: { required },
        state: { required },
        postcode: { required, numeric },
        tele: { required, numeric },
        email: { required, email },
        website: { required, url },
        distance_type: { required },
        seq: { required, numeric },
        loc_page_seq: { required, numeric },
        telephone_display: { required },
        distOffset: { required, numeric },
        comments: { required },
        page_id: { required },
      }
  },
  methods: {
    addLocation() {
       this.$v.$touch();
       if (!this.$v.$invalid) {
        let data = JSON.parse(JSON.stringify(this.add_location))
        let address = data.addr_1 + " " + data.addr_2 + " " + " " + data.city + " " + data.state + " " + data.postcode;

        this.$axios.get('https://maps.googleapis.com/maps/api/geocode/json?address=' + address + '&key=AIzaSyDxQvvTfdRg1tQNVjmLKbIgYOk1TaQEaso')
        .then(response => {
          // handle success
          data.latitude = response.data.results[0].geometry.location.lat
          data.longitude = response.data.results[0].geometry.location.lng

          let coords = "" + data.latitude + "," + data.longitude
          this.$axios.get('https://maps.googleapis.com/maps/api/timezone/json?location=' + coords + '&timestamp=' + Number(new Date().valueOf()/1000) + '&key=AIzaSyDxQvvTfdRg1tQNVjmLKbIgYOk1TaQEaso')
          .then(response => {
            // handle success
            data.timezone = response.data.timeZoneId  

            this.$axios.post('http://testg.salerise.com/site/module/food/update_location?tkn=q0kqLUEtis9ZRLIys7Aw0UQqzixWBWaVDC1ZzC2UZ5QKEtMhIpZTliYGtQA=', data, {
            headers: { 
                'X-Requested-With': 'XMLHttpRequest',
                'Authorization': 'Bearer N0g4MBlO7W6wmSeitiMZwd9GLSlCU7O651yn0gFN'
            },
            }).then(response => {
                Notify.create({
                    message: 'Location Added Sucessfully!',
                    icon: 'cloud_done',
                    color: 'green',
                })
                this.$router.push('/Location')
            }).catch(error => {
                Notify.create({
                    message: 'Location Added UnSuccessfully! Please try refreshing the page or check your internet connection and try again. ',
                    icon: 'announcement',
                    color: 'red',
                })
            })
        })
        })
       } else {
            Notify.create({
                message: 'Location Failed Validation',
                icon: 'warning',
                color: 'red',
            })
       }
    }
  }
}
</script>
