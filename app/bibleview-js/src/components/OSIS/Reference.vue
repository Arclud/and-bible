<!--
  - Copyright (c) 2020 Martin Denham, Tuomas Airaksinen and the And Bible contributors.
  -
  - This file is part of And Bible (http://github.com/AndBible/and-bible).
  -
  - And Bible is free software: you can redistribute it and/or modify it under the
  - terms of the GNU General Public License as published by the Free Software Foundation,
  - either version 3 of the License, or (at your option) any later version.
  -
  - And Bible is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY;
  - without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
  - See the GNU General Public License for more details.
  -
  - You should have received a copy of the GNU General Public License along with And Bible.
  - If not, see http://www.gnu.org/licenses/.
  -->

<template>
  <a class="reference" @click="openLink(link)" :href="link" ref="content"><slot/></a>
</template>

<script>
import {checkUnsupportedProps, useCommon} from "@/composables";
import {addEventFunction} from "@/utils";

export default {
  name: "Reference",
  props: {
    osisRef: {type: String, default: null},
    source: {type: String, default: null},
    type: {type: String, default: null},
  },
  computed: {
    queryParams() {
      if(!this.osisRef && this.$refs.content) {
        return "content=" + encodeURI(this.$refs.content.innerText);
      }
      return "osis=" + encodeURI(this.osisRef)
    },
    link({queryParams}) {
      return `ab-reference://?${queryParams}`
    }
  },
  setup(props) {
    checkUnsupportedProps(props, "type");
    const {strings, ...common} = useCommon();
    function openLink(event, url) {
      addEventFunction(event, () => window.location.assign(url), {title: strings.referenceLink});
    }
    return {openLink, ...common};
  },
}

</script>

<style scoped>
  .reference {
    color: red;
  }
</style>
