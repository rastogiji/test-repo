# [KCGW - Child Account] Dataplane Uptime Dashboard

| Panel Name | Panel Description | Panel Type | Metrics Used |
| ---------- | ----------------- | ---------- | -------- |
| Org Uptime Parameters - 1 means up |  | timeseries | <pre><code>kong_http_requests_total<br></code></pre> <pre><code>kube_deployment_status_replicas_ready<br></code></pre> <pre><code>aws_networkelb_un_healthy_host_count_maximum<br></code></pre> <pre><code>aws_networkelb_rejected_flow_count_maximum<br></code></pre> |
| DP Uptime Parameters - 1 means up |  | timeseries | <pre><code>kong_http_requests_total<br></code></pre> <pre><code>kube_deployment_status_replicas_ready<br></code></pre> <pre><code>aws_networkelb_healthy_host_count_maximum<br></code></pre> <pre><code>aws_networkelb_rejected_flow_count_maximum<br></code></pre> |
| R53 DPG Health (All DPGs under Org) - 1 means UP | This panel shows if the Data Plane Group (DPG) is healthy or not. Any failing probe would mean the downtime. The panel shows all the DPGs under the Org Id. | timeseries | <pre><code>dns_controller_route53_healthchecks_total<br></code></pre> |
| Dataplane CPU Usage |  | gauge | <pre><code>container_cpu_usage_seconds_total<br></code></pre> <pre><code>kube_pod_container_resource_limits<br></code></pre> |
| Dataplane Memory Usage |  | gauge | <pre><code>container_memory_working_set_bytes<br></code></pre> <pre><code>kube_pod_container_resource_limits<br></code></pre> |
| Kong vs NLB connections |  | timeseries | <pre><code>kong_nginx_connections_total<br></code></pre> <pre><code>aws_networkelb_active_flow_count_maximum<br></code></pre> |
| Dataplane Logs Size |  | timeseries | <pre><code>kubelet_container_log_filesystem_used_bytes<br></code></pre> |
| Kong Request latency Buckets |  | timeseries | <pre><code>kong_request_latency_ms_bucket<br></code></pre> |
| DP Pod Recycle Reasons |  | timeseries | <pre><code>kube_pod_status_reason<br></code></pre> <pre><code>kube_pod_container_status_waiting_reason<br></code></pre> |
