# GrokE2Guardian-to-InfluxDB
To use at your own risk ! It's intended to use on pfsense log to send directly to influxDB to monitor it on Graphana

```
patterns = ["^%{SYSLOGTIMESTAMP:timestamp:ts-ansic}%{SPACE}%{NUMBER:ID}%{SPACE}%{IPORHOST:destination:tag}%{SPACE}%{WORD:protocol}[/]%{NUMBER:Unkwon}%{SPACE}%{NUMBER:TargetNumber}%{SPACE}-%{SPACE}%{URIPROTO:urlprotocol:tag}://%{IPORHOST:urlsource:tag}%{SPACE}-%{SPACE}%{WORD:Parent}/%{SPACE}-"]
```

```
[[inputs.logparser]] 
files = ["/var/log/pfblockerng/E2GUARDIAN.log"]
  from_beginning=true
  [inputs.logparser.grok]
    measurement = "dnsbl_log"
    patterns = ["^%{SYSLOGTIMESTAMP:timestamp:ts-ansic}%{SPACE}%{NUMBER:ID}%{SPACE}%{IPORHOST:destination:tag}%{SPACE}%{WORD:protocol}[/]%{NUMBER:Unkwon}%{SPACE}%{NUMBER:TargetNumber}%{SPACE}-%{SPACE}%{URIPROTO:urlprotocol:tag}://%{IPORHOST:urlsource:tag}%{SPACE}-%{SPACE}%{WORD:Parent}/%{SPACE}-"]
    timezone = "Local"
    [inputs.logparser.tags]
      value = "1"
 ```     
      
Tested on a Grok Debugger https://grokdebug.herokuapp.com/ so the input like 


`Oct 18 14:04:13.318     57 10.0.0.68 TCP_MISS/0 0 - https://fonts.googleapis.com - DEFAULT_PARENT/ -`

Output like this

```
{
  "timestamp": [
    [
      "Oct 18 14:04:13.318"
    ]
  ],
  "MONTH": [
    [
      "Oct"
    ]
  ],
  "MONTHDAY": [
    [
      "18"
    ]
  ],
  "TIME": [
    [
      "14:04:13.318"
    ]
  ],
  "HOUR": [
    [
      "14"
    ]
  ],
  "MINUTE": [
    [
      "04"
    ]
  ],
  "SECOND": [
    [
      "13.318"
    ]
  ],
  "SPACE": [
    [
      " ",
      " ",
      " ",
      " ",
      " ",
      " ",
      " ",
      " ",
      " "
    ]
  ],
  "ID": [
    [
      "57"
    ]
  ],
  "BASE10NUM": [
    [
      "57",
      "0",
      "5"
    ]
  ],
  "destination": [
    [
      "10.0.0.68"
    ]
  ],
  "HOSTNAME": [
    [
      "10.0.0.68",
      "fonts.googleapis.com"
    ]
  ],
  "IP": [
    [
      null,
      null
    ]
  ],
  "IPV6": [
    [
      null,
      null
    ]
  ],
  "IPV4": [
    [
      null,
      null
    ]
  ],
  "protocol": [
    [
      "TCP_MISS"
    ]
  ],
  "Unkwon": [
    [
      "0"
    ]
  ],
  "TargetNumber": [
    [
      "5"
    ]
  ],
  "urlprotocol": [
    [
      "https"
    ]
  ],
  "urlsource": [
    [
      "fonts.googleapis.com"
    ]
  ],
  "Parent": [
    [
      "DEFAULT_PARENT"
    ]
  ]
}
```
