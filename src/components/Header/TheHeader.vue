<template>
  <header class="header" :class="{ '--meeting': currentPage === '/' }">
    <!--Back-->
    <div class="header__back" v-if="currentPage === '/detail'">
      <router-link to="/" tag="button">
        <BaseIcon ico="arrow-left"></BaseIcon>
        <span>Назад</span>
      </router-link>
    </div>

    <!--Title-->
    <div
      class="header__title"
      :class="{ '--w-auto': currentPage === '/users', '--w-half': currentPage === '/detail' }"
    >
      <h1 :title="titlePage">
        {{ titlePage }}
      </h1>

      <button
        v-if="currentPage === '/detail'"
        @click="openPopupRedactSession('Редактировать заседание')"
      >
        <BaseIcon ico="redact"></BaseIcon>
      </button>

      <button
        v-if="currentPage === '/users'"
        @click="popupAddObj = !popupAddObj"
      >
        <BaseIcon ico="redact"></BaseIcon>
      </button>
    </div>

    <!--Options-->
    <div class="header__options" v-if="currentPage === '/'">
      <div
        class="header__calendar"
        v-click-outside="hideCalendar"
        :class="{ active: calendar }"
      >
        <button @click="openCalendar">
          <BaseIcon ico="calelndar-2" class-names="icon-calendar"></BaseIcon>
          <span>{{ date.toLocaleDateString() }}</span>
          <BaseIcon ico="arrow-down" class-names="icon-arrow"></BaseIcon>
        </button>
        <v-date-picker v-model="date" v-show="calendar" title-position="left" />
      </div>

      <BaseSelect
        :list="['Зал 11', 'Зал 12', 'Судебный пристав', 'Зал 14', 'Зал 15']"
        class="--small  --firstselect-min --close-text --meeting-list"
        placeholder="Помещение"
      ></BaseSelect>
      
    </div>

    <div class="header__options header__meeting" v-if="currentPage === '/'">
      <button
        class="btn-add --blue"
        @click="openPopupRedactSession('Создание судебного заседания')"
      >
        <BaseIcon ico="plus"></BaseIcon>
        <span>Создать заседание</span>
      </button>
    </div>

    <PopupRedactSession
      :show="popupRedactSession"
      @hidePopup="popupRedactSession = false"
      :title="popupRedactSessionTitle"
    ></PopupRedactSession>

    <!--Notify-->
    <div class="header__notify">
      <button
        v-if="currentPage === '/detail'"
        @click="popupMeetingParticip = !popupMeetingParticip"
      >
        Участники заседания
      </button>
      <TheNotify></TheNotify>
    </div>

    <PopupMeetingParticip
      :show="popupMeetingParticip"
      @hidePopup="popupMeetingParticip = false"
    ></PopupMeetingParticip>

    <PopupAddObj
      :show="popupAddObj"
      @hidePopup="popupAddObj = false"
    ></PopupAddObj>

    <PopupRedactUser
      :show="popupRedactUser"
      @hidePopup="popupRedactUser = false"
    ></PopupRedactUser>
  </header>
</template>

<script>
import { mapState } from "vuex";
import BaseIcon from "@/components/global/BaseIcon";
import BaseSelect from "@/components/global/BaseSelect";
import TheNotify from "@/components/Notify/TheNotify";
import ClickOutside from "vue-click-outside";
import PopupRedactSession from "@/components/Popup/PopupRedactSession";
import PopupMeetingParticip from "@/components/Popup/PopupMeetingParticip";
import PopupAddObj from "@/components/Popup/PopupAddObj";
import PopupRedactUser from "@/components/Popup/PopupRedactUser";

export default {
  name: "TheHeader",
  components: {
    TheNotify,
    BaseIcon,
    BaseSelect,
    PopupRedactSession,
    PopupMeetingParticip,
    PopupAddObj,
    PopupRedactUser
  },
  data() {
    return {
      popupRedactSessionTitle: "Создание судебного заседания",
      popupRedactSession: false,
      popupMeetingParticip: false,
      popupAddObj: false,
      popupRedactUser: false,
      calendar: false,
      date: new Date()
    };
  },
  computed: {
    ...mapState(["titlePage", "currentPage"]),
    currentPage(e) {
      return e.$route.path;
    }
  },
  methods: {
    openCalendar() {
      this.calendar = !this.calendar;
    },
    hideCalendar() {
      this.calendar = false;
    },
    openPopupRedactSession(title) {
      this.popupRedactSessionTitle = title || false;
      this.popupRedactSession = !this.popupRedactSession;
    }
  },
  directives: {
    ClickOutside
  }
};
</script>
