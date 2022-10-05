## Prvisionierung server

```bash
docker-compose up -d
```

## Push some metrics

In grafan query eg: 'some_metric{job="somejob"}'

```powershell
$metrics = "
# TYPE some_metric 
some_metric 42
# TYPE awesomeness_total counter
# HELP awesomeness_total How awesome is this article.
awesomeness_total 99999999
"

Invoke-WebRequest -Uri "http://localhost:9091/metrics/job/somejob/instance/someinstance" -Body $metrics -Method Post
```

```bash
cat <<EOF | curl --data-binary @- http://localhost:9091/metrics/job/somejob/instance/someinstance
# TYPE some_metric gauge
some_metric 42
# TYPE awesomeness_total counter
# HELP awesomeness_total How awesome is this article.
awesomeness_total 99999999
EOF
```

# Windows exporter


Install: https://github.com/prometheus-community/windows_exporter/releases

Dann Test:  http://localhost:9182/metrics

Run this:
```powershell
$promHost = "10.163.242.161:9091"
$promJob = "somejob"
$promInstance = "someinstance"

while (1 -eq 1 ){
  $result = Invoke-WebRequest -Uri "http://localhost:9182/metrics" -Method Get
  Invoke-WebRequest -Uri "http://${promHost}/metrics/job/${promJob}/instance/${promInstance}" -Body $result.Content -Method Post
  start-sleep -seconds 15
}
```

## Dahboard

Import json from here: https://grafana.com/grafana/dashboards/14694-windows-exporter-dashboard/