<template>
  <div
    class="select-block"
    :class="[
      {
        active: openedSelect,
        '--simple': simple,
        '--min': min,
        '--full-width': fullWidth,
        '--focus': focus,
        '--thin': thin,
        '--onlyplaceholder': onlyplaceholder,
        '--firstselect': !selected,
        '--close-text': closeText,
        '--meeting-list': meetingList,
        '--arrow': arrow
      },
      classNames
    ]"
    v-click-outside="hideSelect"
  >
    <input type="text" class="select-block__input" :value="selected" />

    <button type="button" class="select-block__button" @click="selectToggle">

      <!--Placeholder-->
      <div class="select-block__placeholder" v-if="!selected" :style="{top: '50%', transform: 'translateY(-50%)'} " >
        {{ placeholder }}
      </div>

      <div class="select-block__placeholder" v-else :style="{top: '30%', transform: 'translateY(-50%)'}" >
        {{ placeholder }}
      </div>

      <!--Selected-->
      <div class="select-block__selected" v-if="selected" :width="{top: this.widthSelected}">{{ room ? "Помещение" : selected }}</div>

      <!--Arrow-->
      <div class="select-block__arrow">
        <BaseIcon ico="arrow-down"></BaseIcon>
      </div>
    </button>

    <!--List-->
    <div class="select-block__list" v-show="openedSelect">
      <ul>
        <li
          v-for="(item, index) in newList"
          :key="index"
          :class="{ active: selectedIndex === index }"
          @click="selectItem(item, index)"
        >
          {{ item }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import BaseIcon from "@/components/global/BaseIcon";
import ClickOutside from "vue-click-outside";

export default {
  name: "BaseSelect",
  components: { BaseIcon },
  props: {
    widthSelected: {
      type: String
    },
    paddingArchive: {
      type: String
    },
    firstselect: {
      type: Boolean,
      default: false
    },
    hereIsTheThing: {
      type: Boolean,
      default: false
    },
    closeText: {
      type: Boolean,
      default: false
    },
    meetingList: {
      type: Boolean,
      default: false
    },
    thin: {
      type: Boolean,
      default: false
    },
    placeholder: {
      type: String,
      default: ""
    },
    list: {
      type: Array,
      default: () => {
        return [];
      }
    },
    classNames: {
      type: String,
      default: ""
    },
    simple: {
      type: Boolean,
      default: false
    },
    arrow: {
      type: Boolean,
      default: false
    },
    min: {
      type: Boolean,
      default: false
    },
    fullWidth: {
      type: Boolean,
      default: false
    },
    onlyplaceholder: {
      type: Boolean,
      default: false
    },
    selectPlaceholder: {
      type: Boolean,
      default: false
    },
    room: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      selected: this.firstselect ? false : "",
      selectedIndex: this.list.indexOf(this.list[0]),
      openedSelect: false,
      focus: false
    };
  },
  computed: {
    newList(e) {
      return e.list.filter(item => {
        if (item) return item;
      });
    }
  },
  methods: {
    selectToggle() {
      this.openedSelect = !this.openedSelect;
    },
    selectItem(item, index) {
      this.selected = item;
      this.selectedIndex = index;
      this.openedSelect = false;
    },
    hideSelect() {
      this.openedSelect = false;
    }
  },
  directives: {
    ClickOutside
  }
};
</script>
