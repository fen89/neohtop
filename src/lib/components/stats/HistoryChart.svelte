<script lang="ts">
  import { VisXYContainer, VisLine, VisAxis, VisArea } from "@unovis/svelte";
  import { Scale } from "@unovis/ts";

  type DataRecord = {
    date: Date;
    value: number;
  };

  export let data: DataRecord[] = [];

  const formatNumberCompact = (
    value: number,
    maximumFractionDigits: number = 0,
  ) =>
    Intl.NumberFormat("en", {
      maximumFractionDigits,
      notation: "compact",
    }).format(value);

  function getUsageColor(usage: number): string {
    if (usage > 90) return "var(--red)";
    if (usage > 75) return "var(--peach)";
    if (usage > 50) return "var(--yellow)";
    return "var(--blue)";
  }

  const x = (d: DataRecord) => +new Date(d.date);
  const y = (d: DataRecord) => d.value;

  const xScale = Scale.scaleTime();

  const xTickFormat = (value: number) => {
    const now = Date.now();
    const isNow = Math.round((now - value) / 1000) < 1;
    if (isNow) {
      return 0;
    } else {
      return `${Math.round((now - value) / 1000)} seconds ago`;
    }
  };

  const yTickFormat = (i: number) => formatNumberCompact(i, 1);

  $: latestValue = data[data.length - 1]?.value;
  $: chartColor = getUsageColor(latestValue);
</script>

<VisXYContainer
  {data}
  {xScale}
  yDomain={[0, 100]}
  duration={0}
  padding={{ top: 10 }}
  margin={{ bottom: 15, left: 10 }}
  key={data.length}
>
  <VisLine {x} {y} color={chartColor} />
  <VisArea {x} {y} opacity="0.3" color={chartColor} />

  <VisAxis type="x" {x} tickFormat={xTickFormat} minMaxTicksOnly={true} />
  <VisAxis type="y" tickFormat={yTickFormat} />
</VisXYContainer>
