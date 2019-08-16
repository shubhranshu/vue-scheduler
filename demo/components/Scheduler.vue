<template>
  <v-layout class="scheduler">
    <v-flex xs2 class="left">
      Left Pane
    </v-flex>
    <v-flex xs8 class="cal">
      <Table>
        <tbody>
          <tr>
            <td></td>
            <template v-for="(day, index) in header1">
              <td :key="index" :colspan="colspanDate">{{ day }}</td>
            </template>
          </tr>
          <tr>
            <td></td>
            <template v-for="(time, index) in header2">
              <td :key="index">{{ time }}</td>
            </template>
          </tr>
          <template v-for="(resource, index) in resources">
            <tr :key="index">
              <td>{{ resource }}</td>
              <template v-for="(time, index2) in header2">
                <td :key="index2">&nbsp;</td>
              </template>
            </tr>
          </template>
        </tbody>
      </Table>
    </v-flex>
    <v-flex xs2 class="right">
      Left pane
    </v-flex>
  </v-layout>
</template>

<script>
import { DateTime } from 'luxon';
export default {
  props: {
    startDate: {
      type: Object,
      default: () => DateTime.local().set({ hour: 0, minute: 0, second: 0 })
    },
    endDate: {
      type: Object,
      default: () => DateTime.local().plus({ days: 7 })
    },
    mode: {
      type: String,
      default: 'day' // lets only support this mode
    }
  },
  data() {
    return {
      header1: [],
      header2: [],
      colspanDate: 1,
      resources: [],
      displayIntervalMinutes: 30
    };
  },
  computed: {
    totalDays() {
      return '' + this.endDate.diff(this.startDate, 'days').days;
    }
  },
  mounted() {
    for (let i = 0; i < this.totalDays; i++) {
      this.header1.push(this.startDate.plus({ days: i }).toLocaleString(DateTime.DATE_SHORT));
    }
    const intervals = 24 / (this.displayIntervalMinutes / 60);
    this.colspanDate = intervals;
    for (let j = 0; j < this.totalDays * intervals; j++) {
      this.header2.push(this.startDate.plus({ minutes: this.displayIntervalMinutes * j }).toLocaleString(DateTime.TIME_SIMPLE));
    }

    for (let k = 0; k < 50; k++) {
      this.resources.push('Resource ' + k);
    }
  }
};
</script>

<style lang="scss" scoped>
.scheduler {
  .left {
    background-color: green;
  }
  .cal {
    background-color: red;
    overflow: auto;
    &-header {
      height: 40px;
      &-1,
      &-2 {
        height: 20px;
      }
    }
    table {
      td {
        text-align: center;
        border: 1px solid;
      }
    }
  }
  .right {
    background-color: blue;
  }
}
</style>
