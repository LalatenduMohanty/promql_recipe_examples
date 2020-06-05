Query to list all alerts in last 24 hours across all the clusters.

```
sort_desc(
  count by (alertname,namespace,install_minor,current_minor) (
    max by (_id,alertname,namespace) (alerts{alertstate="firing",severity="critical"} offset 24h)
    + on (_id) group_left (install_minor,current_minor)
    0 * topk by (_id) (1, max by (_id,install_minor,current_minor) (
      label_replace(
        label_replace(
          cluster_version{type="cluster",from_version=~"4[.][1-9][.][0-9]*",version=~"4[.][1-9][.][0-9]*"} offset 24h,
          "install_minor", "$1", "from_version", "(4[.][0-9])[.][0-9]*"
        ),
        "current_minor", "$1", "version", "(4[.][0-9])[.][0-9]*"
      )
    ))
  )
)
```