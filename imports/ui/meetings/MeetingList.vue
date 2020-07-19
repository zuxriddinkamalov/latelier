<template>
  <div class="meeting-list">
    <empty-state
      v-if="!meetings || !Array.isArray(meetings) || meetings.length == 0"
      :illustration="emptyIllustration"
      small
      :label="$t('meetings.none')"
    />
    <v-list v-else-if="meetings.length > 0" two-line>
      <template v-for="meeting in meetings">
        <v-list-item :key="meeting._id" @click.stop="select(meeting)">
          <v-list-item-avatar :color="meeting.color" />
          <v-list-item-content>
            <v-list-item-title class="no-wrap">
              <span class="black--text">
                {{ meeting.name }}
              </span>
            </v-list-item-title>
            <v-list-item-subtitle v-if="meeting.project">
              <span class="grey--text text--darken-1 show-desktop">
                <template v-if="meeting.organization">
                  {{ meeting.organization.name }} /
                </template>
                {{ meeting.project.name }}
              </span>
            </v-list-item-subtitle>
            <v-list-item-subtitle v-if="getMeetingInterval(meeting)">
              <div class="meeting-date">
                {{ getMeetingInterval(meeting) }}
              </div>
            </v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
        <v-divider :key="`divider-${meeting._id}`" inset />
      </template>
    </v-list>
  </div>
</template>

<script>
import DatesMixin from "/imports/ui/mixins/DatesMixin";

export default {
  mixins: [DatesMixin],
  props: {
    meetings: {
      type: Array,
      default() {
        return [];
      }
    },
    emptyIllustration: {
      type: String,
      default: "empty"
    }
  },
  methods: {
    select(meeting) {
      this.$emit("select", meeting);
    },
    getMeetingInterval(meeting) {
      if (!meeting) return "";
      return this.displayDateInterval({
        start: meeting.startDate,
        end: meeting.endDate,
        type: "dateWithHours"
      });
    }
  }
};
</script>

<style scoped>
.empty-state {
  padding: 2rem;
}

.meeting-date {
  display: flex;
  align-items: center;
}

.no-wrap {
  white-space: normal;
}
</style>