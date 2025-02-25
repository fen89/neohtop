<script lang="ts">
  import {
    faChartArea,
    faMemory,
    faMicrochip,
  } from "@fortawesome/free-solid-svg-icons";
  import HistoryChart from "./HistoryChart.svelte";
  import PanelHeader from "./PanelHeader.svelte";
  import type { SystemStatsHistory } from "$lib/types";
  import { formatPercentage } from "../../utils";

  export let systemStatsHistory: SystemStatsHistory[] = [];

  $: cpuStats = systemStatsHistory.map((stats) => ({
    date: stats.timestamp,
    value: stats.cpu_usage_avg,
  }));
  $: latestCpuUsage = formatPercentage(cpuStats[cpuStats.length - 1]?.value);

  $: memoryStats = systemStatsHistory.map((stats) => ({
    date: stats.timestamp,
    value: (stats.memory_used / stats.memory_total) * 100,
  }));

  $: latestMemoryUsage = formatPercentage(
    memoryStats[cpuStats.length - 1]?.value,
  );
</script>

<div class="stat-panel">
  <PanelHeader
    icon={faMicrochip}
    title="CPU usage"
    usageValue={latestCpuUsage}
  />
  <div class="stats-content">
    <HistoryChart data={cpuStats} />
  </div>
</div>

<div class="stat-panel">
  <PanelHeader icon={faMemory} title="Memory" usageValue={latestMemoryUsage} />
  <div class="stats-content">
    <HistoryChart data={memoryStats} />
  </div>
</div>

<style>
  .stat-panel {
    min-width: 0;
    background-color: var(--mantle);
    border-radius: 6px;
    padding: 0.75rem;
    display: flex;
    flex-direction: column;
  }

  .stats-content {
    display: flex;
    flex-direction: column;
    gap: 0.4rem;
  }
</style>
