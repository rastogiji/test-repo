# [KCGW - Child Account] Dataplane Uptime Dashboard

| Panel Name | Panel Description | Panel Type | Formulas |
| ---------- | ----------------- | ---------- | -------- |
| Org Uptime Parameters - 1 means up |  | timeseries | <pre><code>min((avg_over_time(
     (
       sum(rate(kong_http_requests_total{orgId=\"\$orgId\", code=~\"5..\", source=\"kong\"}[\$__range])) or vector(0)
       /
       sum(rate(kong_http_requests_total{orgId=\"\$orgId\", source=\"kong\"}[1m]))
     )[1m] < bool 0.05
   )
  )
), (avg_over_time(
     min(kube_deployment_status_replicas_ready{orgId=\"\$orgId\"})[1m] > bool 0
   )
), (avg_over_time(
     sum(aws_networkelb_un_healthy_host_count_maximum{orgId=\"\$orgId\", dimension_AvailabilityZone!~\".*\"}) == bool 0
   [1m])
), avg_over_time(sum(aws_networkelb_rejected_flow_count_maximum{orgId=\"\$orgId\"})[1m] == bool 0)
</code></pre> |
| DP Uptime Parameters - 1 means up |  | timeseries | <pre><code>min((avg_over_time(
     (
       sum(rate(kong_http_requests_total{orgId=\"\$orgId\", clusterName=\"\$clusterName\", namespace=\"\$namespace\", code=~\"5..\", source=\"kong\"}[1m])) or vector(0)
       /
       sum(rate(kong_http_requests_total{orgId=\"\$orgId\", clusterName=\"\$clusterName\", namespace=\"\$namespace\", source=\"kong\"}[1m]))
     )[1m] < bool 0.05
   )
  )
), (avg_over_time(
     min(kube_deployment_status_replicas_ready{orgId=\"\$orgId\", clusterName=\"\$clusterName\", namespace=\"\$namespace\"})[1m] > bool 0
   )
), (avg_over_time(
     sum(aws_networkelb_healthy_host_count_maximum{orgId=\"\$orgId\", clusterName=\"\$clusterName\", tag_gateways_kongcloud_io_controlplane_group_id=\"\$namespace\", dimension_AvailabilityZone!~\".*\"}) > bool 0
   [1m])
), avg_over_time(sum(aws_networkelb_rejected_flow_count_maximum{orgId=\"\$orgId\", clusterName=\"\$clusterName\", tag_gateways_kongcloud_io_controlplane_group_id=\"\$namespace\"})[1m] == bool 0)
</code></pre> |
| R53 DPG Health (All DPGs under Org) - 1 means UP | This panel shows if the Data Plane Group (DPG) is healthy or not. Any failing probe would mean the downtime. The panel shows all the DPGs under the Org Id. | timeseries | <pre><code>min by(dataplane_group_id) (dns_controller_route53_healthchecks_total{healthy=\"true\", org_id=\"\$orgId\"}[5m]) > bool 15</code></pre> |
| Dataplane CPU Usage |  | gauge | <pre><code>sum by (pod)(rate(container_cpu_usage_seconds_total{orgId=\"\$orgId\", clusterName=\"\$clusterName\", namespace=\"\$namespace\",pod=~\"dataplane-.*\", container!=\"\"}[1m]))/sum by (pod)(kube_pod_container_resource_limits{orgId=\"\$orgId\", clusterName=\"\$clusterName\", namespace=\"\$namespace\",pod=~\"dataplane-.*\", unit=\"core\"})</code></pre> |
| Dataplane Memory Usage |  | gauge | <pre><code>sum by (pod)(container_memory_working_set_bytes{orgId=\"\$orgId\", clusterName=\"\$clusterName\", namespace=\"\$namespace\",pod=~\"dataplane-.*\", container!=\"\"})/sum by (pod)(kube_pod_container_resource_limits{orgId=\"\$orgId\", clusterName=\"\$clusterName\", namespace=\"\$namespace\",pod=~\"dataplane-.*\", unit=\"byte\"})</code></pre> |
| Kong vs NLB connections |  | timeseries | <pre><code>(sum(kong_nginx_connections_total{orgId=\"\$orgId\", clusterName=\"\$clusterName\", state=~\"reading|active|waiting|writing\", namespace=\"\$namespace\"})-sum(aws_networkelb_active_flow_count_maximum{orgId=\"\$orgId\", clusterName=\"\$clusterName\", tag_gateways_kongcloud_io_controlplane_group_id=\"\$namespace\"}))/sum(kong_nginx_connections_total{orgId=\"\$orgId\", clusterName=\"\$clusterName\", state=~\"reading|active|waiting|writing\", namespace=\"\$namespace\"})</code></pre> |
| Dataplane Logs Size |  | timeseries | <pre><code>sum by (pod)(increase(kubelet_container_log_filesystem_used_bytes{orgId=\"\$orgId\", clusterName=\"\$clusterName\", namespace= \"\$namespace\", pod=~\"dataplane-.*\"}[1m])) </code></pre> |
| Kong Request latency Buckets |  | timeseries | <pre><code>histogram_quantile(0.9,kong_request_latency_ms_bucket{orgId=\"\$orgId\", clusterName=\"\$clusterName\", pod=\"dataplane-.*\"})</code></pre> |
| DP Pod Recycle Reasons |  | timeseries | <pre><code>sum by (reason, pod)(rate(kube_pod_status_reason{orgId=\"\$orgId\", clusterName=\"\$clusterName\", namespace=\"\$namespace\",pod=~\"dataplane-.*\"}[5m])), sum by (reason,pod,container)(rate(kube_pod_container_status_waiting_reason{orgId=\"\$orgId\", clusterName=\"\$clusterName\", pod=~\"dataplane-.*\"}[5m]))</code></pre> |
