<template>
  <Page
    @loaded="transparentPage"
    backgroundColor="transparent"
    :class="appTheme"
  >
    <GridLayout rows="auto, auto, auto" class="modal">
      <Label class="title" :text="title | L" />
      <StackLayout
        row="1"
        class="dialogListPicker"
        orientation="horizontal"
        horizontalAlignment="center"
      >
        <ListPicker
          @loaded="onLPLoad"
          ref="hrPicker"
          :items="hrsList"
          :selectedIndex="hrIndex"
          @selectedIndexChange="setHrs"
        ></ListPicker>
        <ListPicker
          @loaded="onLPLoad"
          ref="minPicker"
          :items="minsList"
          :selectedIndex="minIndex"
          @selectedIndexChange="setMins"
        ></ListPicker>
      </StackLayout>
      <GridLayout row="2" columns="*, auto, auto" class="actions">
        <Button
          col="1"
          class="text sm"
          :text="'cBtn' | L"
          @tap="$modal.close(false)"
        />
        <Button
          col="2"
          class="text sm"
          :text="action | L"
          @tap="$modal.close(selectedTime)"
        />
      </GridLayout>
    </GridLayout>
  </Page>
</template>

<script>
import { mapState } from "vuex";
import { localize } from "@nativescript/localize";
export default {
  props: ["title", "selectedHr", "selectedMin", "action"],
  data() {
    return {
      hrs: [],
      mins: [],
      selectedHrs: "00",
      selectedMins: "00",
    };
  },
  computed: {
    ...mapState(["icon", "appTheme"]),
    hrsList() {
      let h = [...Array(24).keys()];
      this.hrs = h;
      return h.map((e) => `${e} ${localize("hr")}`);
    },
    minsList() {
      let m = [
        ...new Set([
          ...Array(11).keys(),
          ...Array.from(Array(12), (_, x) => x * 5),
        ]),
      ];
      this.mins = m;
      return m.map((e) => `${e} ${localize("min")}`);
    },
    hrIndex() {
      return this.hrs.indexOf(parseInt(this.selectedHr));
    },
    minIndex() {
      return this.mins.indexOf(parseInt(this.selectedMin));
    },
    selectedTime() {
      return this.selectedHrs + ":" + this.selectedMins;
    },
  },
  methods: {
    onLPLoad({ object }) {
      object.android.setWrapSelectorWheel(true);
    },
    setHrs(args) {
      let hr = "0" + this.hrs[args.object.selectedIndex];
      this.selectedHrs = hr.slice(-2);
    },
    setMins(args) {
      let min = "0" + this.mins[args.object.selectedIndex];
      this.selectedMins = min.slice(-2);
    },
  },
};
</script>
