<template>
    <q-page class="q-pa-sm bg-white">

        <q-resize-observer @resize="onResize"/>

        <div class="row" v-if="!$q.screen.lt.sm">
          <div class="col-lg-4 col-md-4 col-sm-4 col-xs-4">
              <q-card flat bordered>
                <q-tab-panel class="q-pa-none full-height">
                  <div v-if="displayMe" class="q-pa-none full-height" :style="{'height':size['height']-20+'px !important'}">
                        <q-toolbar class="text-black bg-grey-2">
                            <q-item class="q-subtitle-1 q-pl-md">
                              <q-item-section>
                                  <q-item-label lines="1" class="text-h6 text-weight-bold">Locations</q-item-label>
                              </q-item-section>
                            </q-item>

                            <q-space/>
                            <q-item-label flat class="bg-grey text-white" header>{{ locations_list.length }}</q-item-label>
                            <q-btn flat icon="add_box" style="font-size: 1.3em!important" color="grey" @click="$router.push('/AddLocation')"> </q-btn>
                        
                        
                        </q-toolbar>

                        <q-separator></q-separator>

                        <q-toolbar class="text-black">
                            <q-item-label class="text-center q-pa-sm full-width">
                              <q-input dense outlined v-model="search" >
                                <template v-slot:append>
                                  <q-icon name="search"/>
                                </template>
                              </q-input>
                            </q-item-label>
                        </q-toolbar>
                        <q-separator></q-separator>

                        <div>
                           <div class="flex flex-center" style="margin-top: 14rem;">
                              <q-card-section class="text-h5 text-weight-bold">
                                <div>Add Location</div>
                              <div class="row justify-center">
                                <q-btn round flat icon="add_box" color="green" style="font-size: 0.8em!important" @click="$router.push('/AddLocation')"> </q-btn>
                              </div>
                              </q-card-section>
                          </div>
                        </div>
                  </div>
                  
                  <div v-else class="q-pa-none full-height" :style="{'height':size['height']-20+'px !important'}">
                        <q-toolbar class="text-black bg-grey-2">
                            <q-item class="q-subtitle-1 q-pl-md">
                              <q-item-section>
                                  <q-item-label lines="1" class="text-h6 text-weight-bold">Locations</q-item-label>
                              </q-item-section>
                            </q-item>

                            <q-space/>
                            <q-item-label flat class="bg-grey text-white" header>{{ locations_list.length }}</q-item-label>
                            <q-btn flat icon="add_box" style="font-size: 1.3em!important" color="grey" @click="$router.push('/AddLocation')"> </q-btn>
                        
                        
                        </q-toolbar>

                        <q-separator></q-separator>

                        <q-toolbar class="text-black">
                            <q-item-label class="text-center q-pa-sm full-width">
                              <q-input dense outlined v-model="search" >
                                <template v-slot:append>
                                  <q-icon name="search"/>
                                </template>
                              </q-input>
                            </q-item-label>
                        </q-toolbar>
                        <q-separator></q-separator>

                        <div>
                            <div class="q-gutter-md row flex flex-center justify-center" v-if="loading" style="margin-top: 15rem; margin-bottom: 20rem; ">
                              <div>
                                <q-spinner-gears
                                  size="4rem"
                                  color="green"
                                />
                                <q-tooltip :offset="[0, 8]">QSpinnerGears</q-tooltip>
                              </div>
                            </div>

                            <q-list v-else class="">
        
                                <span v-for="(location, index) in getData" :key="index">
                                <q-item v-ripple>
                                    <q-item-section>
                                      <q-item-label>{{ location.addr_1 }}</q-item-label>
                                    </q-item-section>
                                    <q-space/>

                                    <q-btn round flat icon="delete" color="red" @click="deleteLocation(location.p_loc_id)"></q-btn>
                                    <q-btn flat icon="chevron_right" color="green" @click="selected_location=location; update_location=location"> </q-btn>
                                </q-item>
                                <q-separator></q-separator>

                                </span>

                            </q-list>
                            <div class="q-pa-lg flex flex-center" style="relative">
                              <q-pagination
                                    v-model="page"
                                    :min="currentPage" 
                                    :max="Math.ceil(locations_list.length/totalPages)"
                                    :input="true"
                                    input-class="text-orange-10"
                                    style="position: absolute; bottom: 0;">
                              </q-pagination>
                            </div>
                        </div>
                  </div>
                </q-tab-panel>
              </q-card>
          </div>

          <div v-if="!isHidden" class="col-lg-8 q-pl-xs col-md-8 col-sm-8 col-xs-8">
              <q-card bordered flat>
                  <q-toolbar class="text-black bg-grey-2 ">
                      <q-item class="q-subtitle-1 q-pl-md">
                          <q-item-section>
                              <q-item-label lines="1" class="text-h6 text-weight-bold">Location Details</q-item-label>
                          </q-item-section>
                      </q-item>

                      <q-space/>

                      <q-btn @click="isHidden = !isHidden" v-if="isBtn" round flat icon="edit"/>
                  </q-toolbar>
                  <q-separator></q-separator>
                  <q-card-section class="q-pa-sm">
                      <q-list class="row">
                      
                      <q-item class="col-lg-3 col-md-3 col-sm-3 col-xs-3 hidden">
                          <q-item-section>
                          <q-input color="black" v-model="selected_location.id" dense />
                          </q-item-section>
                      </q-item>
                      <q-item class="col-lg-3 col-md-3 col-sm-3 col-xs-3 hidden">
                          <q-item-section>
                          <q-input color="black" v-model="selected_location.p_loc_id" dense />
                          </q-item-section>
                      </q-item>
                      <q-item class="col-lg-3 col-md-3 col-sm-3 col-xs-3 hidden">
                          <q-item-section>
                          <q-input color="black" v-model="selected_location.user_id" dense />
                          </q-item-section>
                      </q-item>
                      <q-item class="col-lg-3 col-md-3 col-sm-3 col-xs-3 hidden">
                          <q-item-section>
                          <q-input color="black" v-model="selected_location.page_id" dense />
                          </q-item-section>
                      </q-item>
                      <q-item class="q-subtitle-1 q-pl-md q-pt-lg">
                          <q-item-section class="text-h6">
                              <q-item-label lines="1">Address</q-item-label>
                          </q-item-section>
                      </q-item>
                      <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                          <q-item-section>
                          <q-input color="black" v-model="selected_location.addr_1" dense label="Address Line 1" readonly/>
                          </q-item-section>
                      </q-item>
                      <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                          <q-item-section>
                          <q-input color="black" v-model="selected_location.addr_2" dense label="Address Line 2" readonly/>
                          </q-item-section>
                      </q-item>
                      <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
                          <q-item-section>
                          <q-input color="black" v-model="selected_location.city" dense label="City" readonly/>
                          </q-item-section>
                      </q-item>
                      <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
                          <q-item-section>
                          <q-input color="black" v-model="selected_location.state" dense label="State" readonly/>
                          </q-item-section>
                      </q-item>
                      <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                          <q-item-section>
                          <q-input color="black" v-model="selected_location.postcode" dense label="Postal Code" readonly/>
                          </q-item-section>
                      </q-item>

                      <q-item class="q-subtitle-1 q-pl-md q-pt-lg">
                          <q-item-section class="text-h6">
                              <q-item-label lines="1">Contact Details</q-item-label>
                          </q-item-section>
                      </q-item>
                      <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                          <q-item-section>
                          <q-input color="black" v-model="selected_location.website" dense label="Website" readonly/>
                          </q-item-section>
                      </q-item>
                      <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
                          <q-item-section>
                          <q-input color="black" v-model="selected_location.email" dense label="Email" readonly/>
                          </q-item-section>
                      </q-item>
                      <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12 ">
                          <q-item-section>
                          <q-input color="black" v-model="selected_location.tele" dense label="Telephone" readonly/>
                          </q-item-section>
                      </q-item>
                      
                      <q-item class="q-subtitle-1 q-pt-lg q-pt-lg">
                          <q-item-section class="text-h6">
                              <q-item-label lines="1">Geolocation</q-item-label>
                          </q-item-section>
                      </q-item>
                      <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                          <q-item-section>
                          <q-input color="black" v-model="selected_location.latitude" dense label="Laititude" readonly/>
                          </q-item-section>
                      </q-item>
                      <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                          <q-item-section>
                          <q-input color="black" v-model="selected_location.longitude" dense label="Longtitude" readonly/>
                          </q-item-section>
                      </q-item>
                      <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                          <q-item-section>
                          <q-input color="black" v-model="selected_location.timezone" dense label="Time Zone" readonly/>
                          </q-item-section>
                      </q-item>

                      <q-item class="q-subtitle-1 q-pt-lg q-pt-lg">
                          <q-item-section class="text-h6">
                              <q-item-label lines="1">Others</q-item-label>
                          </q-item-section>
                      </q-item>
                      <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                          <q-item-section>
                          <q-input color="black" v-model="selected_location.distOffset" dense label="Distance Type" readonly/>
                          </q-item-section>
                      </q-item>
                      <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                          <q-item-section>
                          <q-input color="black" v-model="selected_location.telephone_display" dense label="Telephone Display" readonly/>
                          </q-item-section>
                      </q-item>
                      <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                          <q-item-section>
                          <q-input color="black" v-model="selected_location.distance_type" dense label="Distance Offset" readonly/>
                          </q-item-section>
                      </q-item>
                      <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                          <q-item-section>
                          <q-input color="black" v-model="selected_location.seq" dense label="Sequence" readonly/>
                          </q-item-section>
                      </q-item>
                      <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                          <q-item-section>
                          <q-input color="black" v-model="selected_location.loc_page_seq" dense label="Location Page Sequence" readonly/>
                          </q-item-section>
                      </q-item>
                      <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                          <q-item-section>
                          <q-input color="black" v-model="selected_location.comments" dense label="Comments" readonly/>
                          </q-item-section>
                      </q-item>

                      </q-list>
                  </q-card-section>
              </q-card>
          </div>

          <div v-else class="col-lg-8 q-pl-xs col-md-8 col-sm-8 col-xs-8">
              <q-card bordered flat>
                  <q-toolbar class="text-black bg-grey-2 ">
                      <q-item class="q-subtitle-1 q-pl-md">
                          <q-item-section>
                              <q-item-label lines="1" class="text-h6 text-weight-bold">Location Details</q-item-label>
                          </q-item-section>
                      </q-item>
                  </q-toolbar>
                  <q-separator></q-separator>
                  <q-card-section class="q-pa-sm">
                      <q-list class="row">
                        <q-item class="col-lg-4 col-md-4 col-sm-4 col-xs-4 hidden">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.p_loc_id" dense />
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-4 col-md-4 col-sm-4 col-xs-4 hidden">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.user_id" dense/>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-4 col-md-4 col-sm-4 col-xs-4 hidden">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.page_id" dense/>
                            </q-item-section>
                        </q-item>
                        <q-item class="q-subtitle-1 q-pl-md q-pt-lg">
                            <q-item-section class="text-h6">
                                <q-item-label lines="1">Address</q-item-label>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.addr_1" dense label="Address Line 1"/>
                            <span class="text-red" v-if="$v.update_location.addr_1.$error">Field required</span>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.addr_2" dense label="Address Line 2"/>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.city" dense label="City"/>
                            <span class="text-red" v-if="update_location.city.$error">Field required</span>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.state" dense label="State" />
                            <span class="text-red" v-if="$v.update_location.state.$error">Field required</span>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.postcode" dense label="Postal Code"/>
                            <span class="text-red" v-if="$v.update_location.postcode.$error">Must be a numeric value </span>
                            </q-item-section>
                        </q-item>

                        <q-item class="q-subtitle-1 q-pl-md q-pt-lg">
                            <q-item-section class="text-h6">
                                <q-item-label lines="1">Contact Details</q-item-label>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.website" dense label="Website"/>
                            <span class="text-red" v-if="$v.update_location.website.$error"> URL address required </span>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.email" dense label="Email"/>
                            <span class="text-red" v-if="$v.update_location.email.$error">Email address required </span>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.tele" dense label="Telephone"/>
                            <span class="text-red" v-if="$v.update_location.tele.$error">Must be a numeric value </span>
                            </q-item-section>
                        </q-item>
                        
                        <q-item class="q-subtitle-1 q-pt-lg q-pt-lg">
                            <q-item-section class="text-h6">
                                <q-item-label lines="1">Geolocation</q-item-label>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.latitude" dense label="Laititude" readonly/>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.longitude" dense label="Longtitude" readonly/>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.timezone" dense label="Time Zone" readonly/>
                            </q-item-section>
                        </q-item>

                        <q-item class="q-subtitle-1 q-pt-lg q-pt-lg">
                            <q-item-section class="text-h6">
                                <q-item-label lines="1">Others</q-item-label>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.distOffset" dense label="Distance Type"/>
                            <span class="text-red" v-if="$v.update_location.distOffset.$error">Numeric value required </span>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.telephone_display" dense label="Telephone Display"/>
                            <span class="text-red" v-if="$v.update_location.telephone_display.$error">Field required </span>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.distance_type" dense label="Distance Offset"/>
                            <span class="text-red" v-if="$v.update_location.distance_type.$error"> Field required </span>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.seq" dense label="Sequence"/>
                            <span class="text-red" v-if="$v.update_location.seq.$error">Numeric value required</span>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.loc_page_seq" dense label="Location Page Sequence"/>
                            <span class="text-red" v-if="$v.update_location.loc_page_seq.$error">Numeric value required </span>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.comments" dense label="Comments"/>
                            <span class="text-red" v-if="$v.update_location.comments.$error"> Field required </span>
                            </q-item-section>
                        </q-item>
                      </q-list>
                  </q-card-section>
                  <q-card-actions align="right">
                      <q-btn class="text-capitalize bg-positive text-white" @click="updateLocation()">Update</q-btn>
                      <q-btn class="text-capitalize bg-negative text-white" @click="isHidden = !isHidden">Cancel</q-btn>
                  </q-card-actions>
              </q-card>
          </div>
        </div>

        <div v-else>
          <div v-if="Object.keys(selected_location).length==0">
            <q-card flat bordered>
              <div v-if="displayMe" class="q-pa-none full-height" :style="{'height':size['height']-20+'px !important'}">
                <q-toolbar class="text-black bg-grey-2">
                    <q-item class="q-subtitle-1 q-pl-md">
                      <q-item-section>
                          <q-item-label lines="1" class="text-h6 text-weight-bold">Locations</q-item-label>
                      </q-item-section>
                    </q-item>

                    <q-space/>
                    <q-item-label flat class="bg-green text-white" header>{{ locations_list.length }}</q-item-label>
                    <q-btn flat icon="add_box" style="font-size: 1.3em!important" color="green" @click="$router.push('/AddLocation')"> </q-btn>
                
                
                </q-toolbar>

                <q-separator></q-separator>

                <q-toolbar class="text-black">
                    <q-item-label class="text-center q-pa-sm full-width">
                      <q-input dense outlined v-model="search" >
                        <template v-slot:append>
                          <q-icon name="search"/>
                        </template>
                      </q-input>
                    </q-item-label>
                </q-toolbar>
                <q-separator></q-separator>

                <div>
                    <div class="flex flex-center" style="padding: 10rem 0px; ">
                      <q-card-section class="text-h5 text-weight-bold">
                        <div>Add Location</div>
                      <div class="row justify-center">
                        <q-btn round flat icon="add_box" color="green" style="font-size: 0.8em!important" @click="$router.push('/AddLocation')"> </q-btn>
                      </div>
                      </q-card-section>
                  </div>
                </div>
              </div>
                  
              <div v-else class="q-pa-none full-height" :style="{'height':size['height']-20+'px !important'}">
                <q-toolbar class="text-black bg-grey-2">
                    <q-item class="q-subtitle-1 q-pl-md">
                      <q-item-section>
                          <q-item-label lines="1" class="text-h6 text-weight-bold">Locations</q-item-label>
                      </q-item-section>
                    </q-item>

                    <q-space/>
                    <q-item-label flat class="bg-green text-white" header>{{ locations_list.length }}</q-item-label>
                    <q-btn flat icon="add_box" style="font-size: 1.3em!important" color="green" @click="$router.push('/AddLocation')"> </q-btn>
                
                
                </q-toolbar>

                <q-separator></q-separator>

                <q-toolbar class="text-black">
                    <q-item-label class="text-center q-pa-sm full-width">
                      <q-input dense outlined v-model="search" >
                        <template v-slot:append>
                          <q-icon name="search"/>
                        </template>
                      </q-input>
                    </q-item-label>
                </q-toolbar>
                <q-separator></q-separator>

                <div>
                    <div class="q-gutter-md row flex flex-center justify-center" v-if="loading" style="margin-top: 10rem; margin-bottom: 20rem; ">
                      <div>
                        <q-spinner-gears
                          size="4rem"
                          color="green"
                        />
                        <q-tooltip :offset="[0, 8]">QSpinnerGears</q-tooltip>
                      </div>
                    </div>

                    <q-list v-else class="">

                        <span v-for="(location, index) in getData" :key="index">
                        <q-item v-ripple>
                            <q-item-section>
                              <q-item-label>{{ location.addr_1 }}</q-item-label>
                            </q-item-section>
                            <q-space/>

                            <q-btn round flat icon="delete" color="red" @click="deleteLocation(location.p_loc_id)"></q-btn>
                            <q-btn flat icon="chevron_right" color="green" @click="selected_location=location; update_location=location"> </q-btn>
                        </q-item>
                        <q-separator></q-separator>

                        </span>

                    </q-list>
                    <div class="q-pa-lg flex flex-center" style="relative">
                      <q-pagination
                            v-model="page"
                            :min="currentPage" 
                            :max="Math.ceil(locations_list.length/totalPages)"
                            :input="true"
                            input-class="text-orange-10"
                            style="position: absolute; bottom: 0;">
                      </q-pagination>
                    </div>
                </div>
              </div>
            </q-card>
          </div>

          <transition v-else
                      appear
                      enter-active-class="animated bounceInRight"
          >
          
            <div v-if="!isHidden" class="col-lg-8 q-pl-xs col-md-8 col-sm-8 col-xs-8">
                <q-card bordered flat>
                    <q-toolbar class="text-black bg-grey-2 ">
                        <q-item class="q-subtitle-1 q-pl-md">
                            <q-item-section>
                                <q-item-label lines="1" class="text-h6 text-weight-bold">Location Details</q-item-label>
                            </q-item-section>
                        </q-item>

                        <q-space/>

                        <q-btn @click="isHidden = !isHidden" v-if="isBtn" round flat icon="edit"/>
                        <q-btn round flat icon="keyboard_backspace" @click="selected_location={}"/>
                    </q-toolbar>
                    <q-separator></q-separator>
                    <q-card-section class="q-pa-sm">
                        <q-list class="row">
                        
                        <q-item class="col-lg-3 col-md-3 col-sm-3 col-xs-3 hidden">
                            <q-item-section>
                            <q-input color="black" v-model="selected_location.id" dense />
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-3 col-md-3 col-sm-3 col-xs-3 hidden">
                            <q-item-section>
                            <q-input color="black" v-model="selected_location.p_loc_id" dense />
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-3 col-md-3 col-sm-3 col-xs-3 hidden">
                            <q-item-section>
                            <q-input color="black" v-model="selected_location.user_id" dense />
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-3 col-md-3 col-sm-3 col-xs-3 hidden">
                            <q-item-section>
                            <q-input color="black" v-model="selected_location.page_id" dense />
                            </q-item-section>
                        </q-item>
                        <q-item class="q-subtitle-1 q-pl-md q-pt-lg">
                            <q-item-section class="text-h6">
                                <q-item-label lines="1">Address</q-item-label>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="selected_location.addr_1" dense label="Address Line 1" readonly/>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="selected_location.addr_2" dense label="Address Line 2" readonly/>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="selected_location.city" dense label="City" readonly/>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="selected_location.state" dense label="State" readonly/>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="selected_location.postcode" dense label="Postal Code" readonly/>
                            </q-item-section>
                        </q-item>

                        <q-item class="q-subtitle-1 q-pl-md q-pt-lg">
                            <q-item-section class="text-h6">
                                <q-item-label lines="1">Contact Details</q-item-label>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="selected_location.website" dense label="Website" readonly/>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="selected_location.email" dense label="Email" readonly/>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12 ">
                            <q-item-section>
                            <q-input color="black" v-model="selected_location.tele" dense label="Telephone" readonly/>
                            </q-item-section>
                        </q-item>
                        
                        <q-item class="q-subtitle-1 q-pt-lg q-pt-lg">
                            <q-item-section class="text-h6">
                                <q-item-label lines="1">Geolocation</q-item-label>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="selected_location.latitude" dense label="Laititude" readonly/>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="selected_location.longitude" dense label="Longtitude" readonly/>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="selected_location.timezone" dense label="Time Zone" readonly/>
                            </q-item-section>
                        </q-item>

                        <q-item class="q-subtitle-1 q-pt-lg q-pt-lg">
                            <q-item-section class="text-h6">
                                <q-item-label lines="1">Others</q-item-label>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="selected_location.distOffset" dense label="Distance Type" readonly/>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="selected_location.telephone_display" dense label="Telephone Display" readonly/>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="selected_location.distance_type" dense label="Distance Offset" readonly/>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="selected_location.seq" dense label="Sequence" readonly/>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="selected_location.loc_page_seq" dense label="Location Page Sequence" readonly/>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="selected_location.comments" dense label="Comments" readonly/>
                            </q-item-section>
                        </q-item>

                        </q-list>
                    </q-card-section>
                </q-card>
            </div>

            <div v-else class="col-lg-8 q-pl-xs col-md-8 col-sm-8 col-xs-8">
                <q-card bordered flat>
                    <q-toolbar class="text-black bg-grey-2 ">
                        <q-item class="q-subtitle-1 q-pl-md">
                            <q-item-section>
                                <q-item-label lines="1" class="text-h6 text-weight-bold">Location Details</q-item-label>
                            </q-item-section>
                        </q-item>

                        <q-space/>
                        <q-btn round flat icon="keyboard_backspace" @click="selected_location={}"/>
                    </q-toolbar>
                    <q-separator></q-separator>
                    <q-card-section class="q-pa-sm">
                      <q-list class="row">
                        <q-item class="col-lg-4 col-md-4 col-sm-4 col-xs-4 hidden">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.p_loc_id" dense />
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-4 col-md-4 col-sm-4 col-xs-4 hidden">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.user_id" dense/>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-4 col-md-4 col-sm-4 col-xs-4 hidden">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.page_id" dense/>
                            </q-item-section>
                        </q-item>
                        <q-item class="q-subtitle-1 q-pl-md q-pt-lg">
                            <q-item-section class="text-h6">
                                <q-item-label lines="1">Address</q-item-label>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.addr_1" dense label="Address Line 1"/>
                            <span class="text-red" v-if="$v.update_location.addr_1.$error">Field required</span>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.addr_2" dense label="Address Line 2"/>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.city" dense label="City"/>
                            <span class="text-red" v-if="update_location.city.$error">Field required</span>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.state" dense label="State" />
                            <span class="text-red" v-if="$v.update_location.state.$error">Field required</span>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.postcode" dense label="Postal Code"/>
                            <span class="text-red" v-if="$v.update_location.postcode.$error">Must be a numeric value </span>
                            </q-item-section>
                        </q-item>

                        <q-item class="q-subtitle-1 q-pl-md q-pt-lg">
                            <q-item-section class="text-h6">
                                <q-item-label lines="1">Contact Details</q-item-label>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.website" dense label="Website"/>
                            <span class="text-red" v-if="$v.update_location.website.$error"> URL address required </span>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.email" dense label="Email"/>
                            <span class="text-red" v-if="$v.update_location.email.$error">Email address required </span>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.tele" dense label="Telephone"/>
                            <span class="text-red" v-if="$v.update_location.tele.$error">Must be a numeric value </span>
                            </q-item-section>
                        </q-item>
                        
                        <q-item class="q-subtitle-1 q-pt-lg q-pt-lg">
                            <q-item-section class="text-h6">
                                <q-item-label lines="1">Geolocation</q-item-label>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.latitude" dense label="Laititude" readonly/>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.longitude" dense label="Longtitude" readonly/>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.timezone" dense label="Time Zone" readonly/>
                            </q-item-section>
                        </q-item>

                        <q-item class="q-subtitle-1 q-pt-lg q-pt-lg">
                            <q-item-section class="text-h6">
                                <q-item-label lines="1">Others</q-item-label>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.distOffset" dense label="Distance Type"/>
                            <span class="text-red" v-if="$v.update_location.distOffset.$error">Numeric value required </span>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.telephone_display" dense label="Telephone Display"/>
                            <span class="text-red" v-if="$v.update_location.telephone_display.$error">Field required </span>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.distance_type" dense label="Distance Offset"/>
                            <span class="text-red" v-if="$v.update_location.distance_type.$error"> Field required </span>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.seq" dense label="Sequence"/>
                            <span class="text-red" v-if="$v.update_location.seq.$error">Numeric value required</span>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.loc_page_seq" dense label="Location Page Sequence"/>
                            <span class="text-red" v-if="$v.update_location.loc_page_seq.$error">Numeric value required </span>
                            </q-item-section>
                        </q-item>
                        <q-item class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
                            <q-item-section>
                            <q-input color="black" v-model="update_location.comments" dense label="Comments"/>
                            <span class="text-red" v-if="$v.update_location.comments.$error"> Field required </span>
                            </q-item-section>
                        </q-item>
                      </q-list>
                    </q-card-section>
                    <q-card-actions align="right">
                        <q-btn class="text-capitalize bg-positive text-white" @click="updateLocation()">Update</q-btn>
                        <q-btn class="text-capitalize bg-negative text-white" @click="pageRefresh()">Cancel</q-btn>
                    </q-card-actions>
                </q-card>
            </div>
          </transition>
        </div>
    </q-page>
</template>

<script>
import { Dialog } from 'quasar'
import { Notify } from 'quasar'
import { required, email, numeric, url } from 'vuelidate/lib/validators'

const headers = {
    'X-Requested-With': 'XMLHttpRequest',
    'Authorization': 'Bearer N0g4MBlO7W6wmSeitiMZwd9GLSlCU7O651yn0gFN'
}
export default {
  name: "Location",
  computed:{
    getData(){
        return 	this.filteredLocation.slice((this.page-1)*this.totalPages,(this.page-1)*this.totalPages+this.totalPages)
    },
    mobileGetData(){
        return 	this.filteredLocation.slice((this.page-1)*this.mobiletotalPages,(this.page-1)*this.mobiletotalPages+this.mobiletotalPages)
    },
    filteredLocation() {
      return this.locations_list.filter((location) => {
          return location.addr_1 .match(this.search)
      })
    }
  },
  data() {
    return {
      loading: true,
      isHidden: false,
      isBtn: true,
      displayMe: false,
      search: "",
      page: 1,
      currentPage:1,
      nextPage: null,
      totalPages:19,
      mobiletotalPages:10,
      tab: 'all',
      size: {},
      locations_list: [],
      selected_location: {},
      update_location: {
        addr_1: '',
        addr_2: '',
        city: '',
        state: '',
        postcode: '',
        tele: '',
        email: '',
        website: '',
        distance_type: '',
        seq: '',
        loc_page_seq: '',
        telephone_display: '',
        distOffset: '',
        comments: '',
      }
    }
  },
  validations: {
      update_location: {
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
      }
  },
  methods: {
    onResize(size) {
      this.size = size;
    },
    getlocation() {
      this.$axios({
      method: 'post',
      url: 'http://testg.salerise.com/site/module/food/get_locations?tkn=q0kqLUEtis9ZRLIys7Aw0UQqzixWBWaVDC1ZzC2UZ5QKEtMhIpZTliYGtQA=',
      headers,
      }).then(response => { 
          if (response.data.status === 'fail') {
            this.loading = false
            Notify.create({
                message: 'Something went wrong. Please try refreshing the page or check your internet connection.',
                icon: 'annoucement',
                color: 'red',
            })
          }
          else {
            if (response.data.status !== 'fail') {
              this.loading = false
              const locationData = response.data.data
              if (locationData == null) {
                  this.displayMe = true
                  this.isBtn = false
              } else {
                  this.locations_list = locationData
              }
            }
          }
      })
    },
    updateLocation() {
       this.$v.$touch();
       if (!this.$v.$invalid) {
        let data = JSON.parse(JSON.stringify(this.update_location))
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
              this.$axios({
                  method: 'post',
                  url: 'http://testg.salerise.com/site/module/food/update_location?tkn=q0kqLUEtis9ZRLIys7Aw0UQqzixWBWaVDC1ZzC2UZ5QKEtMhIpZTliYGtQA=', 
                  data: data,
                  headers,
                  }).then(response => {
                      if (response.data.status === 'fail') {
                          Notify.create({
                            message: 'Something went wrong. Please try refreshing the page or check your internet connection.',
                            icon: 'announcement',
                            color: 'red',
                          })
                      } else {
                        if (response.data.status !== 'fail') {
                            Notify.create({
                              message: 'Location Updated Successfully',
                              icon: 'cloud_done',
                              color: 'green',
                            })
                        }
                        this.getlocation()
                      }
                      window.location.reload();
                  })
          }).catch(error => {
              Notify.create({
                  message: 'Empty Record, Please select location',
                  icon: 'warning',
                  color: 'red',
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
    },
    deleteLocation(id) {
      const data = {
        "id": id
      }
      Dialog.create({ 
        title: 'Confirm',
        message: 'Would you like to delete this location?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.$axios({
          method: 'post',
          url: 'http://testg.salerise.com/site/module/food/delete_location?tkn=q0kqLUEtis9ZRLIys7Aw0UQqzixWBWaVDC1ZzC2UZ5QKEtMhIpZTliYGtQA=', 
          data, headers,
        }).then(response => {
          if (response.data.status === 'fail') {
            Notify.create({
                message: 'Something went wrong. Please try refreshing the page or check your internet connection.',
                icon: 'annoucement',
                color: 'red',
            })
          }
          else {
            if (response.data.status !== 'fail') {
              Notify.create({
                message: 'Location Deleted Successfully',
                icon: 'cloud_done',
                color: 'green',
              })
            }
            window.location.reload();
          }
        })
      })
    },
    pageRefresh() {
      window.location.reload();
    }
  },
  created() {
    this.getlocation()
  }
}
</script>

<style scoped>

</style>
