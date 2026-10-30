# PromQL Queries Used with Explanations

Run these expressions in Prometheus (`http://localhost:9090`) or Grafana Explore with the Prometheus data source selected.

## 1. CPU usage percentage

```promql
100 - (avg by (instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
```

**Explanation:** `node_cpu_seconds_total` is a counter of CPU time. `rate(...[5m])` calculates the per-second rate over five minutes. Filtering `mode="idle"` measures the idle share. The query averages idle CPU across cores, then subtracts it from 100 to estimate CPU usage percentage per instance.

**Unit:** Percent (%).

## 2. Memory usage percentage

```promql
100 * (1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes))
```

**Explanation:** `MemAvailable` estimates memory available to applications without swapping. Dividing it by total memory gives the available fraction. Subtracting from 1 and multiplying by 100 gives used memory percentage.

**Unit:** Percent (%).

## 3. Total memory

```promql
node_memory_MemTotal_bytes
```

**Explanation:** Returns the total physical memory detected by Node Exporter.

**Unit:** Bytes. In Grafana, select a bytes/IEC unit for readable display.

## 4. Available memory

```promql
node_memory_MemAvailable_bytes
```

**Explanation:** Returns the estimated memory available for starting new applications without swapping.

**Unit:** Bytes.

## 5. Root filesystem usage percentage

```promql
100 * (1 - (node_filesystem_avail_bytes{mountpoint="/",fstype!="rootfs"} / node_filesystem_size_bytes{mountpoint="/",fstype!="rootfs"}))
```

**Explanation:** Selects the root mount point `/`. It divides available filesystem bytes by total filesystem size, subtracts the available fraction from 1, and converts the result to a percentage.

## 6. Scrape target health

```promql
up
```

**Explanation:** Prometheus automatically creates the `up` metric for each scrape target. A value of `1` means the last scrape succeeded; `0` means the scrape failed.

**Unit:** Boolean-like status (1 = UP, 0 = DOWN).


