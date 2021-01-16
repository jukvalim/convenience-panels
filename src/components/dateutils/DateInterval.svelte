<script lang="ts">
  import format from "date-fns-tz/format";
  import add from "date-fns/add";
  import intervalToDuration from "date-fns/intervalToDuration";
  import formatDuration from "date-fns/formatDuration";
  import isValid from "date-fns/isValid";
  import differenceInCalendarDays from "date-fns/differenceInCalendarDays";
  import differenceInCalendarWeeks from "date-fns/differenceInCalendarWeeks";
  import differenceInCalendarMonths from "date-fns/differenceInCalendarMonths";
  import { dateFormat } from "./constants";

  let fromDate = new Date();
  fromDate.setHours(0, 0, 0, 0);
  let toDate = fromDate;

  const eventDate = (event, defDate: Date): Date => {
    const d = new Date(event.currentTarget.value);
    if (!isValid(d)) {
      return defDate;
    }
    d.setHours(0, 0, 0, 0);
    return d;
  };

  let inclusiveRange = true;

  $: toForDifferenceCalc = inclusiveRange ? add(toDate, {days: 1}): toDate;
  $: duration = intervalToDuration({ start: fromDate, end: toForDifferenceCalc });
  $: dayDifference = differenceInCalendarDays(toForDifferenceCalc, fromDate);
  $: weekDifference = differenceInCalendarWeeks(toForDifferenceCalc, fromDate);
  $: monthDifference = differenceInCalendarMonths(toForDifferenceCalc, fromDate);
</script>

<div>
  (week {format(fromDate, "I")})
  <input
    type="date"
    value="{format(fromDate, dateFormat)}"
    on:change="{(event) => (fromDate = eventDate(event, fromDate))}" />
  to
  <input
    type="date"
    value="{format(toDate, dateFormat)}"
    on:change="{(event) => (toDate = eventDate(event, toDate))}" />
  (week {format(toDate, "I")})

  <div id="inclusive">
    <label for="inclusive-range" title="Is the interval inclusive or not? E.g is Monday to Tuesday one or two days.">
      Inclusive
    </label>
    <input id="inclusive-range"
      type="checkbox"
      checked="{inclusiveRange}"
      on:change="{(event) => (inclusiveRange = event.currentTarget.checked )}" />
  </div>

  <h3>{formatDuration(duration)}</h3>
  {#if dayDifference > 6}
    <p>
      {dayDifference} in days
      {#if weekDifference >= 2}
        | {weekDifference} in weeks
      {/if}
      {#if monthDifference > 12}
        | {monthDifference} in months
      {/if}
    </p>
  {/if}
</div>

<style>
  div {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  div#inclusive {
    flex-direction: row;
    padding-top: 10px;
  }

  div#inclusive > input {
    margin: 2px;
  }
</style>
