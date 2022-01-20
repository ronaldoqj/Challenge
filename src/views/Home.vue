<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated class="glossy">
      <q-toolbar>
        <!-- <q-btn
          flat
          dense
          round
          @click="leftDrawerOpen = !leftDrawerOpen"
          aria-label="Menu"
          icon="o_menu"
        /> -->

        <q-toolbar-title>
          Challenge
        </q-toolbar-title>

        <div>
          <q-btn
            flat
            :to="{name:'Login'}"
            dense
            round
            aria-label="Menu"
            icon="logout"
          />
        </div>
      </q-toolbar>
    </q-header>

    <!-- <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
      class="bg-grey-2"
    >
      <q-list>
        <q-item-label header>Shifts</q-item-label>
        <q-item :to="{name: 'Home'}">
          <q-item-section avatar>
            <q-icon name="o_school" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Home</q-item-label>
            <q-item-label caption>description</q-item-label>
          </q-item-section>
        </q-item>
        <q-item :to="{name: 'About'}">
          <q-item-section avatar>
            <q-icon name="o_code" />
          </q-item-section>
          <q-item-section>
            <q-item-label>About</q-item-label>
            <q-item-label caption>description</q-item-label>
          </q-item-section>
        </q-item>
      </q-list>
    </q-drawer> -->

    <q-page-container>
      <!-- <HelloWorld /> -->
      <!-- <router-view/> -->
      <div class="container__content">
        <div class="left">
          <div>
            <q-select
              color="indigo"
              filled
              v-model="rangeShifts.model"
              :options="rangeShifts.options"
              :label="rangeShifts.label"
            >
              <template v-slot:prepend>
                <q-icon name="event" />
              </template>
            </q-select>
          </div>
          <div class="item__shift"
          v-for="shift in shifts" :key="shift.key" @click="selectedShift(shift)">{{shift.title}}</div>
        </div>
        <div class="right">Right</div>
      </div>
    </q-page-container>
  </q-layout>
</template>

<!-- template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template -->

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import { ref, onMounted, toRefs, computed, watch, reactive } from 'vue'
import { date } from 'quasar'

export default {
  name: 'Home',
  components: {
    // HelloWorld
  },
  setup () {
    /** To use i18n on setup, not necessary on data(not composition), in data only need us $t to access i18n */
    const state = reactive({
      rangeShifts: {
        label: 'Select the day range',
        model: 2,
        options: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
      },
      shifts: [
        // {
        //   id: '1',
        //   title: '2022-01-17 15:00:00 - early',
        //   content: {}
        // },
        // {
        //   id: '2',
        //   title: '2022-01-17 21:00:00 - late',
        //   content: {}
        // },
        // {
        //   id: '3',
        //   title: '2022-01-17 21:00:00 - night',
        //   content: {}
        // }
      ]
    })
    /**
     * Computeds
     */
    const exampleComputed = computed(() => {
      return true
    })
    /**
     * Methods
     */

    const generateStartShifts = () => {
      const currentDate = Date.now()
      const currentYear = date.formatDate(currentDate, 'YYYY')
      const currentMonth = date.formatDate(currentDate, 'MM')
      const currentDay = date.formatDate(currentDate, 'DD')
      const dateEarlyStart = date.buildDate({ year: currentYear, month: currentMonth, date: currentDay, hours: 7, minutes: 0, seconds: 0 })
      const dateEarlyEnd = date.buildDate({ year: currentYear, month: currentMonth, date: currentDay, hours: 14, minutes: 0, seconds: 0 })
      const dateLateStart = date.buildDate({ year: currentYear, month: currentMonth, date: currentDay, hours: 15, minutes: 0, seconds: 0 })
      const dateLateEnd = date.buildDate({ year: currentYear, month: currentMonth, date: currentDay, hours: 22, minutes: 0, seconds: 0 })
      // const dateNightStart = date.buildDate({ year: currentYear, month: currentMonth, date: currentDay, hours: 23, minutes: 0, seconds: 0 })
      // const dateNightEnd = date.buildDate({ year: currentYear, month: currentMonth, date: currentDay, hours: 6, minutes: 0, seconds: 0 })
      console.log('dateEarlyStart', dateEarlyStart)

      const newShiftLate = {
        date: currentDate,
        title: date.formatDate(currentDate, 'YYYY-MM-DD') + ' 23:00:00 - late',
        content: {}
      }
      const newShiftNight = {
        date: currentDate,
        title: date.formatDate(currentDate, 'YYYY-MM-DD') + ' 07:00:00 - night',
        content: {}
      }

      if (date.isBetweenDates(currentDate, dateEarlyStart, dateEarlyEnd)) {
        state.shifts.push(newShiftLate)
        state.shifts.push(newShiftNight)
        console.log(state.shifts)
      }
      if (date.isBetweenDates(currentDate, dateLateStart, dateLateEnd)) {
        state.shifts.push(newShiftNight)
      }
    }

    const generateNextShifts = () => {
      const newDate = Date.now()
      // const formattedDate = date.formatDate(newDate, 'YYYY-MM-DD HH:mm:ss')
      let clonedDate = date.clone(newDate)
      for (let i = 1; i <= state.rangeShifts.model; i++) {
        clonedDate = date.addToDate(clonedDate, { days: i })
        const newShiftEarly = {
          date: clonedDate,
          title: date.formatDate(clonedDate, 'YYYY-MM-DD') + ' 15:00:00 - early',
          content: {
            name: 'Name of person',
            age: 'birthday',
            office: 'job',
            hiringDate: '01-01-2022',
            hobbies: 'something you like to do',
            description: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip.'
          }
        }
        const newShiftLate = {
          date: clonedDate,
          title: date.formatDate(clonedDate, 'YYYY-MM-DD') + ' 23:00:00 - late',
          content: {
            name: 'Name of person',
            age: 'birthday',
            office: 'job',
            hiringDate: '01-01-2022',
            hobbies: 'something you like to do',
            description: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip.'
          }
        }
        const newShiftNight = {
          date: clonedDate,
          title: date.formatDate(clonedDate, 'YYYY-MM-DD') + ' 07:00:00 - night',
          content: {
            name: 'Name of person',
            age: 'birthday',
            office: 'job',
            hiringDate: '01-01-2022',
            hobbies: 'something you like to do',
            description: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip.'
          }
        }
        state.shifts.push(newShiftEarly)
        state.shifts.push(newShiftLate)
        state.shifts.push(newShiftNight)
      }
    }

    const selectedShift = (shift) => {
      console.log('shift clicked', shift)
    }

    const getLeftColumn = () => {
      // const timeStamp = Date.now()
      // const formattedDate = date.formatDate(timeStamp, 'YYYY-MM-DD HH:mm:ss')
      // console.log('Date-Now:', formattedDate)
      generateStartShifts()
      generateNextShifts()
    }

    const init = () => {
      state.shifts = []
      console.log('init')
      getLeftColumn()
      return true
    }
    watch(
      () => state.rangeShifts.model,
      () => {
        init()
      }
    )
    onMounted(() => {
      init()
    })
    return { ...toRefs(state), init, selectedShift, exampleComputed, getLeftColumn, leftDrawerOpen: ref(false) }
  }
}
</script>

<style lang="scss">
@import '@/styles/quasar.variables.scss';

.container__content {
  width: 100%;
  display: flex;
  &>div {
    padding: 20px;
  }
  .left {
    border: solid 1px red;
    .item__shift {
      font-size: 1.2em;
      margin: 10px 0;
      transition: ease 0.4ms;
      cursor: pointer;
      &:hover {
        color: indigo;
      }
    }
  }
  .right {
    border: solid 1px greenyellow;
  }
}

</style>
