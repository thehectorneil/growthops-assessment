<template>
  <q-page>
    <div class="header">
      <div class="district text-center q-pa-lg">
        <span class="text-uppercase district-text">District Manager</span>
      </div>
    </div>

    <div class="row">
      <div class="col-xs-12 col-sm-1 col-md-2" v-show="!$q.screen.lt.md"></div>
      <div class="col-xs-12 col-sm-10 col-md-8">
        <div class="row q-pt-xl q-pb-md">
          <div class="q-pa-sm col-xs-12 col-md-3">
            <span>Filter by Age:</span>
            <q-select
              dense
              outlined
              v-model="model"
              :options="options"
              option-value="id"
              option-lable="label"
              @update:model-value="selectFilter(model)"
            />
          </div>
        </div>

        <q-separator />

        <div
          class="q-pt-md row items-start q-gutter-xl q-pb-xl"
          :class="$q.screen.lt.md ? 'justify-center' : ''"
        >
          <q-card class="my-card q-pb-xl" flat v-for="pl in people" :key="pl">
            <q-card-section horizontal class="q-mt-md">
              <q-card-section class="col-3">
                <q-img
                  class="justify-center"
                  style="width: 75px !important"
                  src="~/assets/userphoto.png"
                />
              </q-card-section>
              <q-card-section class="q-pt-xs">
                <div class="text-h6 q-mt-sm q-mb-md text-uppercase">
                  {{ pl.name }}
                </div>
                <div class="text-caption">Email: {{ pl.email }}</div>
                <div class="text-caption">Mobile: {{ pl.phone }}</div>
                <div class="text-caption">Company: {{ pl.company }}</div>
                <div class="text-caption">
                  Address:
                  {{
                    pl.address.suite +
                    ", " +
                    pl.address.street +
                    ", " +
                    pl.address.city +
                    ", " +
                    pl.address.zipcode
                  }}
                </div>
                <div class="text-caption">Website: {{ pl.website }}</div>
                <div class="text-caption">Age: {{ pl.age }}</div>
              </q-card-section>
            </q-card-section>
          </q-card>
        </div>

        <q-separator />
        <div class="row q-pb-lg">
          <div class="q-pt-lg" :class="$q.screen.lt.md ? 'q-pl-xl' : ''">
            <span>Date</span>
            <div class="row">
              <div class="col-12 flex">
                <q-input
                  outlined
                  v-model="text"
                  placeholder="DD/MM/YYYY"
                  class="q-pr-md"
                />
                <q-btn
                  text-color="white"
                  style="background-color: #d41444"
                  label="Convert"
                  @click="clickFormat(text)"
                />
              </div>
            </div>
          </div>
        </div>

        <div class="row"></div>
      </div>
      <div class="col-xs-12 col-sm-1 col-md-2" v-show="!$q.screen.lt.md"></div>
    </div>
  </q-page>
</template>

<script setup>
import { defineComponent, ref, onMounted } from "vue";
import axios from "axios";
import moment from "moment";

const model = ref({
  id: 1,
  label: "All",
});
const leftDrawerOpen = ref(false);
const options = ref([
  { id: 1, label: "All" },
  { id: 2, label: "20 and below" },
  { id: 3, label: "21 to 39" },
  { id: 4, label: "40 and above" },
]);

const people = ref([]);
const text = ref("");

onMounted(() => {
  getPeople();
  countMe();
});

const countMe = () => {
  for (let i = 1; i <= 100; i++) {
    const a = i / 3;
    const b = i / 5;

    const x = Number.isInteger(a);
    const y = Number.isInteger(b);

    if (x && y) {
      console.log("Foo & Bar");
    } else if (x) {
      console.log("Foo");
    } else if (y) {
      console.log("Bar");
    } else {
      console.log(i);
    }
  }
};

const selectFilter = (e) => {
  getPeople(e.id);
};

const getPeople = async (id) => {
  const response = await axios({
    method: "GET",
    baseURL: `http://www.mocky.io/v2/5d73bf3d3300003733081869`,
  });
  // console.log(response.data);

  const peeps = response.data;

  if (id == 2) {
    const results = peeps.filter((obj) => {
      return obj.age <= 20;
    });

    people.value = results;
  } else if (id == 3) {
    const results = peeps.filter((obj) => {
      return obj.age >= 21 && obj.age <= 39;
    });

    people.value = results;
  } else if (id == 4) {
    const results = peeps.filter((obj) => {
      return obj.age >= 40;
    });

    people.value = results;
  } else {
    people.value = peeps;
  }
};

const clickFormat = (text) => {
  console.log(text);
  const date = moment(text, "DD/MM/YYYY");

  alert(
    date.isValid()
      ? moment(text).format("YYYY-DD-MM")
      : "You input incorrect date format."
  );
};
</script>
<style scoped>
.district-text {
  font-size: 25px !important;
}

.header {
  background-color: #fbfbf4;
}

.my-card {
  width: 100%;
  max-width: 390px;
  box-shadow: rgba(0, 0, 0, 0.16) 0px 10px 36px 0px,
    rgba(0, 0, 0, 0.06) 0px 0px 0px 1px !important;
}
</style>
