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
    return { value: tz, label: `${tz} timezone` };
  });
  const selectedTz = { value: timeZone, label: timeZone };

  const dateUpdater = function () {
    dt = new Date();
    setTimeout(dateUpdater, 1000);
  };
  dateUpdater();

  const tzFmt = function (dt: Date, wantedFormat: string) {
    return format(dt, wantedFormat, tzOptions);
  };
  const dtFmt = function (dt: Date) {
    return tzFmt(zonedTime, dateTimeFormat);
  };
  const dateFmt = function (dt: Date) {
    return tzFmt(zonedTime, dateFormat);
  };
</script>

<div>
  <div id="select-wrapper">
    <Select
      items="{timeZones}"
      selectedValue="{selectedTz}"
      on:select="{(event) => {
        timeZone = event.detail.value;
      }}" />
  </div>

  <h3>{dtFmt(dt)}</h3>
  <h3>{tzFmt(dt, "EEEE")}, Week {tzFmt(dt, "I")}</h3>
</div>

<style>
  div#select-wrapper {
    max-width: 300px;
    margin-left: auto;
    margin-right: auto;
  }
</style>
