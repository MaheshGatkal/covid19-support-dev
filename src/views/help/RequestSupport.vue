<template>
  <div>
    <div class="container mt-4">
      <div class="animated fadeIn">
        <div class="row">
          <div class="col-sm-12">
            <div class="card">
              <div class="card-body">
                <div class="row">
                  <div class="col-sm-12">
                    <div v-if="error" class="alert alert-danger">{{error}}</div>
                    <div
                      v-if="status==='submitted'"
                      class="alert alert-success"
                    >Your request submitted successfully.</div>
                    <h4 class="mb-4 text-primary">What support you need?</h4>
                  </div>
                </div>
                <div class="row">
                  <div class="col-sm-4">
                    <fieldset role="group" class="b-form-group form-group-cat">
                      <div role="group" class>
                        <b-form-group label="Category of the support" label-for="basicSelect" :label-cols="6">
                        <b-form-select
                          id="basicSelect"
                          :plain="true" 
                          @change="showCategory($event)"
                          :options="donation_categories"
                          value="Please select"
                          v-model="form.request.category"
                        ></b-form-select>
                      </b-form-group>
                      </div>
                    </fieldset>
                  </div>
                  <div class="col-sm-8">
                    <fieldset role="group" class="b-form-group form-group-cat">
                      <div role="group" class>
                        <input
                          id="helptitle"
                          type="text"
                          placeholder="Short intro of what type of support need"
                          class="form-control"
                          v-model="form.request.title"
                        />
                      </div>
                    </fieldset>
                  </div>
                </div>
                <div class="row">
                  <div class="col-sm-12">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <textarea
                          id="helptitle"
                          type="text"
                          placeholder="Description of what you need"
                          class="form-control"
                          rows="3"
                          v-model="form.request.detail"
                        />
                      </div>
                    </fieldset>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col-sm-12">
            <div class="card">
              <div class="card-header">
                <div>
                  <strong>Requestor details</strong>
                </div>
              </div>
              <div class="card-body">
                <div class="row">
                  <div class="col-sm-12">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <label for="requestfor">Requesting for</label>
                        <div id="requestfor" role="radiogroup" tabindex="-1" class>
                          <div class="custom-control custom-radio custom-control-inline">
                            <input
                              type="radio"
                              id="requestfor1"
                              name="requestfor"
                              value="self"
                              checked="checked"
                              class="custom-control-input"
                              v-model="form.requesting_for"
                            />
                            <label for="requestfor1" class="custom-control-label">Self</label>
                          </div>
                          <div class="custom-control custom-radio custom-control-inline">
                            <input
                              type="radio"
                              id="requestfor2"
                              name="requestfor"
                              value="other"
                              class="custom-control-input"
                              v-model="form.requesting_for"
                            />
                            <label for="requestfor2" class="custom-control-label">On behalf others</label>
                          </div>
                        </div>
                      </div>
                    </fieldset>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col-sm-12">
            <div class="card">
              <div class="card-header">
                <div>
                  <strong>Details of person requiring support</strong>
                </div>
              </div>
              <div class="card-body">
                <div class="row">
                  <div class="col-sm-12">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <label for="fullname">Full Name</label>
                        <input
                          id="fullname"
                          type="text"
                          placeholder="Full name of the person required support"
                          class="form-control"
                          v-model="form.contact.name"
                          :class="{ 'is-invalid': submitted && $v.form.contact.name.$error }"
                        />
                        <div v-if="submitted && $v.form.contact.name.$error" class="invalid-feedback">
                              <span v-if="!$v.form.contact.name.required">Name is required</span>
							                <span v-if="(!$v.form.contact.name.validName) && ($v.form.contact.name.required)">Name is invalid</span>
                        </div>
                      </div>
                    </fieldset>
                  </div>
                </div>
                <div class="row">
                  <div class="col-sm-6">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <label for="phonenumber">Phone Number</label>
                        <input
                          id="phonenumber"
                          type="text"
                          placeholder="Phone number"
                          class="form-control"
                          v-model="form.contact.phone"
                          :class="{ 'is-invalid': submitted && $v.form.contact.phone.$error }"
                        />
                        <div v-if="submitted && $v.form.contact.phone.$error" class="invalid-feedback">
                              <span v-if="!$v.form.contact.phone.required">Phone Numer is required</span>
							                <span v-if="(!$v.form.contact.phone.validPhoneNo) && ($v.form.contact.phone.required)">Phone Numer is invalid</span>
                        </div>
                      </div>
                    </fieldset>
                  </div>
                  <div class="col-sm-6">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <label for="email">Email</label>
                        <input
                          id="email"
                          type="text"
                          placeholder="Email"
                          class="form-control"
                          v-model="form.contact.email"
                          :class="{ 'is-invalid': submitted && $v.form.contact.email.$error }" 
                        />
                        <div v-if="submitted && $v.form.contact.email.$error" class="invalid-feedback">
                              <span v-if="!$v.form.contact.email.required">Email is required</span>
                              <span v-if="!$v.form.contact.email.email">Email is invalid</span>
                        </div>
                      </div>
                    </fieldset>
                  </div>
                </div>
			          <div class="row">
                  <div class="col-sm-6">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <label for="address">Address</label>
                        <input
                          id="houseNo"
                          type="text"
                          placeholder="House Number"
                          class="form-control"
                          v-model="form.contact.houseNo"
                          :class="{ 'is-invalid': submitted && $v.form.contact.houseNo.$error }" 
                        />
                        <div v-if="submitted && $v.form.contact.houseNo.$error" class="invalid-feedback">
                              <span v-if="!$v.form.contact.houseNo.numeric">Valid House Number is required</span>
                        </div>
                      </div>
                    </fieldset>
                  </div>
                  <div class="col-sm-6">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <div class="marginPadding"></div>
                        <input
                          id="streetName"
                          type="text"
                          placeholder="Street Name"
                          class="form-control"
                          v-model="form.contact.streetName"
                          :class="{ 'is-invalid': submitted && $v.form.contact.streetName.$error }" 
                        />
                        <div v-if="submitted && $v.form.contact.streetName.$error" class="invalid-feedback">
                              <span v-if="!$v.form.contact.streetName.required">Street Name is required</span>
                        </div>
                      </div>
                    </fieldset>
                  </div>
                </div>
                <div class="row">
                  <div class="col-sm-6">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <input
                          id="postCode"
                          type="text"
                          placeholder="Postal Code"
                          class="form-control"
                          v-model="form.contact.postCode"
                        />
                      </div>
                    </fieldset>
                  </div>
                  <div class="col-sm-6">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <button
                        type="button"
                        class="btn btn-primary"
                        @click="validateSelfRequest"
                      >Validate</button>
                      </div>
                    </fieldset>
                  </div>
                </div>
                <div class="row" v-if="seenSelf">
                  <div class="col-sm-12">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <label for="address">Full Address</label>
                        <input
                          id="address"
                          type="text"
                          :readonly="true"
                          class="form-control"
                          v-model="form.contact.address"
                        />
                      </div>
                    </fieldset>
                  </div>
				        </div>
                <div class="row" v-if="seenSelf">
                  <div class="col-sm-6">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <label for="town">Postal Town</label>
                        <input
                          id="town"
                          type="text"
                          :readonly="true"
                          class="form-control"
                          v-model="form.contact.town"
                        />
                      </div>
                    </fieldset>
                  </div>
                  <div class="col-sm-6">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <label for="city">City</label>
                        <input
                          id="city"
                          type="text"
                          :readonly="true"
                          class="form-control"
                          v-model="form.contact.city"
                        />
                      </div>
                    </fieldset>
                  </div>
                </div>
				        <div class="row" v-if="seenSelf">
                  <div class="col-sm-6">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <label for="country">Country</label>
                        <input
                          id="country"
                          type="text"
                          :readonly="true"
                          class="form-control"
                          v-model="form.contact.country"
                        />
                      </div>
                    </fieldset>
                  </div>
                  <div class="col-sm-6">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <label for="postCode">Postal Code</label>
                        <input
                          id="postCode"
                          type="text"
                          :readonly="true"
                          class="form-control"
                          v-model="form.contact.postCode"
                        />
                      </div>
                    </fieldset>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="row" v-if="form.requesting_for === 'other'">
          <div class="col-sm-12">
            <div class="card">
              <div class="card-header">
                <div>
                  <strong>Details of person submitting request</strong>
                </div>
              </div>
              <div class="card-body">
                <div class="row">
                  <div class="col-sm-12">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <label for="fullname">Full Name</label>
                        <input
                          id="fullname"
                          type="text"
                          placeholder="Full name of the person required support"
                          class="form-control"
                          v-model="form.requestor.name"
                          :class="{ 'is-invalid': submitted && $v.form.requestor.name.$error }"
                        />
                        <div v-if="submitted && $v.form.requestor.name.$error" class="invalid-feedback">
                              <span v-if="!$v.form.requestor.name.required">Name is required</span>
							                <span v-if="(!$v.form.requestor.name.validName) && ($v.form.requestor.name.required)">Name is invalid</span>
                        </div>
                      </div>
                    </fieldset>
                  </div>
                </div>
                <div class="row">
                  <div class="col-sm-6">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <label for="phonenumber">Phone Number</label>
                        <input
                          id="phonenumber"
                          type="text"
                          placeholder="Phone number"
                          class="form-control"
                          v-model="form.requestor.phone"
                          :class="{ 'is-invalid': submitted && $v.form.requestor.phone.$error }"
                        />
                        <div v-if="submitted && $v.form.requestor.phone.$error" class="invalid-feedback">
                              <span v-if="!$v.form.requestor.phone.required">Phone Numer is required</span>
							                <span v-if="(!$v.form.requestor.phone.validPhoneNo) && ($v.form.requestor.phone.required)">Phone Numer is invalid</span>
                        </div>
                      </div>
                    </fieldset>
                  </div>
                  <div class="col-sm-6">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <label for="email">Email</label>
                        <input
                          id="email"
                          type="text"
                          placeholder="Email"
                          class="form-control"
                          v-model="form.requestor.email"
                          :class="{ 'is-invalid': submitted && $v.form.requestor.email.$error }" 
                        />
                        <div v-if="submitted && $v.form.requestor.email.$error" class="invalid-feedback">
                              <span v-if="!$v.form.requestor.email.required">Email is required</span>
                              <span v-if="!$v.form.requestor.email.email">Email is invalid</span>
                        </div>
                      </div>
                    </fieldset>
                  </div>
                </div>
			          <div class="row">
                  <div class="col-sm-6">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <label for="address">Address</label>
                        <input
                          id="houseNo"
                          type="text"
                          placeholder="House Number"
                          class="form-control"
                          v-model="form.requestor.houseNo"
                          :class="{ 'is-invalid': submitted && $v.form.requestor.houseNo.$error }" 
                        />
                        <div v-if="submitted && $v.form.requestor.houseNo.$error" class="invalid-feedback">
                              <span v-if="!$v.form.requestor.houseNo.numeric">Valid House Number is required</span>
                        </div>
                      </div>
                    </fieldset>
                  </div>
                  <div class="col-sm-6">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <div class="marginPadding"></div>
                        <input
                          id="streetName"
                          type="text"
                          placeholder="Street Name"
                          class="form-control"
                          v-model="form.requestor.streetName"
                          :class="{ 'is-invalid': submitted && $v.form.requestor.streetName.$error }" 
                        />
                        <div v-if="submitted && $v.form.requestor.streetName.$error" class="invalid-feedback">
                              <span v-if="!$v.form.requestor.streetName.required">Street Name is required</span>
                        </div>
                      </div>
                    </fieldset>
                  </div>
                </div>
                <div class="row">
                  <div class="col-sm-6">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <input
                          id="postCode"
                          type="text"
                          placeholder="Postal Code"
                          class="form-control"
                          v-model="form.requestor.postCode"
                        />
                      </div>
                    </fieldset>
                  </div>
                  <div class="col-sm-6">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <button
                        type="button"
                        class="btn btn-primary"
                        @click="validateOtherRequest"
                      >Validate</button>
                      </div>
                    </fieldset>
                  </div>
                </div>
                <div class="row" v-if="seenOther">
                  <div class="col-sm-12">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <label for="address">Full Address</label>
                        <input
                          id="address"
                          type="text"
                          :readonly="true"
                          class="form-control"
                          v-model="form.requestor.address"
                        />
                      </div>
                    </fieldset>
                  </div>
				        </div>
                <div class="row" v-if="seenOther">
                  <div class="col-sm-6">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <label for="town">Postal Town</label>
                        <input
                          id="town"
                          type="text"
                          :readonly="true"
                          class="form-control"
                          v-model="form.requestor.town"
                        />
                      </div>
                    </fieldset>
                  </div>
                  <div class="col-sm-6">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <label for="city">City</label>
                        <input
                          id="city"
                          type="text"
                          :readonly="true"
                          class="form-control"
                          v-model="form.requestor.city"
                        />
                      </div>
                    </fieldset>
                  </div>
                </div>
				        <div class="row" v-if="seenOther">
                  <div class="col-sm-6">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <label for="country">Country</label>
                        <input
                          id="country"
                          type="text"
                          :readonly="true"
                          class="form-control"
                          v-model="form.requestor.country"
                        />
                      </div>
                    </fieldset>
                  </div>
                  <div class="col-sm-6">
                    <fieldset role="group" class="b-form-group form-group">
                      <div role="group" class>
                        <label for="postCode">Postal Code</label>
                        <input
                          id="postCode"
                          type="text"
                          :readonly="true"
                          class="form-control"
                          v-model="form.requestor.postCode"
                        />
                      </div>
                    </fieldset>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col-sm-12">
            <div class="card">
              <div class="card-body">
                <div class="row">
                  <div class="col-sm-8">
                    <div>
                      <p>By submitting, I am agreeing the T&C and sharing all the above information with the support team.</p>
                    </div>
                  </div>
                  <div class="col-sm-4">
                    <div class="text-right mr-4">
                      <button
                        type="button"
                        class="btn btn-primary"
                        @click="submitRequest"
                      >Agree & Submit Request</button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import firebase from "firebase";
import { mapGetters } from "vuex";
import axios from "axios";
import { required, email, numeric } from "vuelidate/lib/validators";

const validName = function (name) {
    return (name.length >= 2 && /^(?=.{1,50}$)[a-z]+(?:['_.\s][a-z]+)*$/i.test(name));
}

const validPhoneNo = function (phone) {
    return (/^[\+]?[(]?[0-9]{3}[)]?[-\s\.]?[0-9]{3}[-\s\.]?[0-9]{4,6}$/im.test(phone));
}

export default {
  data() {
    return {
      seenSelf: false,
      seenOther: false,
      shouldDisabled: false,
      showCatOther: false,
      submitted: false,
      donation_categories: [
        "General",
        "Food",
        "Groceries",
        "Toys",
        "Books",
        "Furniture",
        "Cloths",
        "Medical supplies",
        "Electronics",
        "Other"
      ],
      form: {
        request: {
          category: "General",
          title: "",
          detail: "",
          status: "new"
        },
        picked_up_by: "",
        picked_up_on: new Date(),
        assigned_groups: [],
        tags: [],
        requesting_for: "self",
        contact: {
          name: "",
          address: "",
          phone: "",
          email: "",
          houseNo: "",
          streetName: "",
          town: "",
          city: "",
          country: "",
          postCode: ""
        },
        requestor: {
          name: "",
          address: "",
          phone: "",
          email: "",
          houseNo: "",
          streetName: "",
          town: "",
          city: "",
          country: "",
          postCode: ""
        }
      },
      error: null,
      status: "new"
    };
  },
  computed: {
    ...mapGetters({
      user: "user"
    })
  },
  validations: {
         form: {
            contact: {
                email: { required, email },
                name: { required, validName},
                phone: { required, validPhoneNo },
                houseNo: { numeric },
                streetName: { required }               
             },
             requestor: {
                email: { required, email },
                name: { required, validName},
                phone: { required, validPhoneNo },
                houseNo: { numeric },
                streetName: { required }               
             }
         }    
  },
  methods: {
    showCategory: function (event) {
      if (event == "Other")
      this.showCatOther= true;
      else
      this.showCatOther= false;
    },
    validateSelfRequest: function () {
      axios.get("https://api.postcodes.io/postcodes/"+this.form.contact.postCode)
      .then((response)  =>  {
        this.seenSelf=true;
        this.form.contact.address=this.form.contact.houseNo+", "+this.form.contact.streetName;
        this.form.contact.town=response.data.result.admin_ward;
        this.form.contact.city=response.data.result.european_electoral_region;
        this.form.contact.country=response.data.result.country;
        this.form.contact.postCode=response.data.result.postcode;
      }, (error)  =>  {
        this.seenSelf=false;
        this.form.contact.houseNo="";
        this.form.contact.streetName="";
        this.error = "Invalid PostCode";
        this.status = "error";
      })
    },
    validateOtherRequest: function () {
      axios.get("https://api.postcodes.io/postcodes/"+this.form.requestor.postCode)
      .then((response)  =>  {
        this.seenOther=true;
        this.form.requestor.address=this.form.requestor.houseNo+", "+this.form.requestor.streetName;
        this.form.requestor.town=response.data.result.admin_ward;
        this.form.requestor.city=response.data.result.european_electoral_region;
        this.form.requestor.country=response.data.result.country;
        this.form.requestor.postCode=response.data.result.postcode;
      }, (error)  =>  {
        alert("Invalid PostCode");
        this.seenOther=false;
        this.form.contact.houseNo="";
        this.form.contact.streetName="";
      })
    },
    submitRequest() {
      this.submitted = true;
      // stop here if form is invalid
      this.$v.$touch();
      if (this.$v.$invalid) {
         return;
      }

      if (this.user.loggedIn && this.user.data) {
        this.form.user_displayName =
          this.user.data.displayName || this.user.data.email;
        this.form.user_email = this.user.data.email;
      }

      this.form.verified = false;
      this.form.upvotes = [];
      this.form.downvotes = [];
      this.form.timestamp = new Date();
     var db = firebase.firestore();
     db.collection("support_requests")
        .add(this.form)
        .then(docRef => {
          this.status = "submitted";
          this.error = null;
          setTimeout(() => {
            this.status = "new";
            this.error = null;
          }, 5 * 1000);
        })
        .catch(error => {
          this.error = error;
          this.status = "error";
        });
    }
  }
};

</script>

<style>
.marginPadding {
   margin-bottom: 1.79rem !important;
}

.form-group-cat {
    margin-bottom: 0.5rem !important;
}
</style>