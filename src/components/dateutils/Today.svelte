<script lang="ts">
  import format from "date-fns-tz/format";
  import utcToZonedTime from "date-fns-tz/utcToZonedTime";
  import { listTimeZones } from "timezone-support";
  import Select from "svelte-select";
  import { dateFormat, dateTimeFormat } from "./constants";

  let dt = new Date();
  let timeZone = Intl.DateTimeFormat().resolvedOptions().timeZone;
  $: tzOptions = { timeZone };
  $: zonedTime = utcToZonedTime(dt, timeZone);
  const timeZones = listTimeZones().map((tz) => {
    return { value: tz, label: `${tz}` };
  });
  const selectedTz = { value: timeZone, label: timeZone };

  const dateUpdater = function () {
    dt = new Date();
    setTimeout(dateUpdater, 1000);
  };
  dateUpdater();

  const tzFmt = function (d: Date, wantedFormat: string) {
    return format(d, wantedFormat, tzOptions);
  };
  const dtFmt = function (d: Date) {
    return tzFmt(d, dateTimeFormat);
  };
</script>

<div>
  <label for="tz-select">Timezone</label>
  <div id="select-wrapper">
    <Select
      items="{timeZones}"
      selectedValue="{selectedTz}"
      inputAttributes="{{id: 'tz-select'}}"
      listAutoWidth="{false}"
      on:select="{(event) => {
        timeZone = event.detail.value;
      }}" />
  </div>

  <h3>{dtFmt(zonedTime)}</h3>
  <h3>{tzFmt(zonedTime, "EEEE")}, Week {tzFmt(zonedTime, "I")}</h3>
</div>

<style>
  div#select-wrapper {
    max-width: 300px;
    margin-left: auto;
    margin-right: auto;
  }
</style>
