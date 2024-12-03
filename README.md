# [KCGW - Child Account] KGO
	| Panel Name | Panel Description | Panel Type | Formulas |
	| :---------- | :----------------- | :---------- | :-------- |
	| KGO Uptime | This panel shows if there was an active leader  pod for KGO available in the cluster  | stat | group(leader_election_master_status{orgId="$orgId", clusterName="$clusterName", namespace="gateway-operator-system"}) |
	| Open FDs | This panel tells you how many file descriptors are currently open by the process | timeseries | process_open_fds{orgId="$orgId", clusterName="$clusterName", namespace="gateway-operator-system"} |
	| Reconciler Error Rate | This panel shows the Error rate encountered by KGO by each controller type | timeseries | sum by (controller)(rate(controller_runtime_reconcile_errors_total{orgId="$orgId", clusterName="$clusterName", namespace="gateway-operator-system"}[5m])) |
	| Active Workers | This panels shows how many active worker process were present at any given point of time | timeseries | sum by (controller)(controller_runtime_active_workers{orgId="$orgId", clusterName="$clusterName", namespace="gateway-operator-system"}) |
	| # of Goroutines | This panel tell you how many goroutines were running at a given time | timeseries | go_goroutines{orgId="$orgId", clusterName="$clusterName", namespace="gateway-operator-system"} |
	| GC Duration | This panel tell you how long the Go garbage Collector took to clean up used memory | timeseries | go_gc_duration_seconds{orgId="$orgId", clusterName="$clusterName", namespace="gateway-operator-system", quantile="1"} |
	| REST Client Requests | This panel tells you how many requests were made by KGO to the API Server grouped by response code and request method | timeseries | sum by (code, method)(rate(rest_client_requests_total{orgId="$orgId", clusterName="$clusterName", namespace="gateway-operator-system"}[5m])) |
	| Workqueue Depth | This panel tell you the depth of the Controller's Workqueue | timeseries | sum by (controller)(workqueue_depth{orgId="$orgId", clusterName="$clusterName", namespace="gateway-operator-system"}) |
	| Workqueue Work Duration | This panel tell you how many seconds it takes to process an item from the workqueue | heatmap | sum by (le)(increase(workqueue_work_duration_seconds_bucket{orgId="$orgId", clusterName="$clusterName", namespace="gateway-operator-system"}[5m])) |
	| Reconcile Duration | This panel tells you how long in seconds it takes for KGO to complete the reconciliation. | heatmap | sum by (le)(increase(controller_runtime_reconcile_time_seconds_bucket{orgId="$orgId", clusterName="$clusterName", namespace="gateway-operator-system"}[5m])) |
	| Total Reconciliations | This panel tells you the rate of reconciliation operations performed grouped by their result | timeseries | sum by (result)(round(increase(controller_runtime_reconcile_total{orgId="$orgId", clusterName="$clusterName", namespace="gateway-operator-system"}[5m]))) |
	| ---------- | ----------------- | ---------- | -------- |
	
