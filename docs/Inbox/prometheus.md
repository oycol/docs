###  部署

config配置文件

```yaml
# my global config
global:
  scrape_interval: 5s
  evaluation_interval: 30s
  # scrape_timeout is set to the global default (10s).

  external_labels:
    monitor: codelab
    foo: bar

#rule_files:
#  - "first.rules"
#  - "my/*.rules"
scrape_configs:
  - job_name: prometheus
      static_configs:
      - targets: ["localhost:9090", "localhost:9191"]

```

- 简单配置

```yaml
global:                                                  
  scrape_interval: 5s                                    
  evaluation_interval: 30s                               
  # scrape_timeout is set to the global default (10s).   
                                                         
#rule_files:                                             
#  - "first.rules"                                       
#  - "my/*.rules"                                        
scrape_configs:                                          
  - job_name: prometheus                                 
    static_configs:                                      
      - targets: ["localhost:9090","localhost:9100"]     
                                                         
  - job_name: pve                                        
    static_configs:                                      
      - targets: ["192.168.2.20:9100"]                   
                                                         
  - job_name: laptop                                     
    static_configs:                                      
      - targets: ["192.168.2.30:9100"]                   
```



单项参数含义

| 名称            | 含义                 | 备注 |
| --------------- | -------------------- | ---- |
| `scrape_config` | 一个定时采集器的参数 |      |
|                 |                      |      |
|                 |                      |      |

### 