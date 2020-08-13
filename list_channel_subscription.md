count by (channel) (
  topk by (_id) (1, cluster_version_available_updates)
  + on(_id) group_left(version)
  topk by (_id) (1, 0 * cluster_version{type="current",version=~"4.*"})
)
