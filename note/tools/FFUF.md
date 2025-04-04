---
tags:
  - 工具
  - 模糊测试
---

## 安装

ffuf为kali自带

# 参数

HTTP OPTIONS:  
  -H                  Header `"Name: Value"`, separated by colon. Multiple -H flags are accepted.  
  -X                  HTTP method to use  
  -b                  Cookie data `"NAME1=VALUE1; NAME2=VALUE2"` for copy as curl functionality.  
  -d                  POST data  
  -ignore-body        Do not fetch the response content. (default: false)  
  -r                  Follow redirects (default: false)  
  -recursion          Scan recursively. Only FUZZ keyword is supported, and URL (-u) has to end in it. (default: false)  
  -recursion-depth    Maximum recursion depth. (default: 0)  
  -recursion-strategy Recursion strategy: "default" for a redirect based, and "greedy" to recurse on all matches (default: default)  
  -replay-proxy       Replay matched requests using this proxy.  
  -timeout            HTTP request timeout in seconds. (default: 10)  
  -u                  Target URL  
  -x                  Proxy URL (SOCKS5 or HTTP). For example: http://127.0.0.1:8080 or socks5://127.0.0.1:8080  
  
GENERAL OPTIONS:  
  -V                  Show version information. (default: false)  
  -ac                 Automatically calibrate filtering options (default: false)  
  -acc                Custom auto-calibration string. Can be used multiple times. Implies -ac  
  -c                  Colorize output. (default: false)  
  -config             Load configuration from a file  
  -maxtime            Maximum running time in seconds for entire process. (default: 0)  
  -maxtime-job        Maximum running time in seconds per job. (default: 0)  
  -p                  Seconds of `delay` between requests, or a range of random delay. For example "0.1" or "0.1-2.0"  
  -rate               Rate of requests per second (default: 0)  
  -s                  Do not print additional information (silent mode) (default: false)  
  -sa                 Stop on all error cases. Implies -sf and -se. (default: false)  
  -se                 Stop on spurious errors (default: false)  
  -sf                 Stop when > 95% of responses return 403 Forbidden (default: false)  
  -t                  Number of concurrent threads. (default: 40)  
  -v                  Verbose output, printing full URL and redirect location (if any) with the results. (default: false)  


MATCHER OPTIONS:  匹配
  -mc                 Match HTTP status codes, or "all" for everything. (default: 200,204,301,302,307,401,403,405)  
  -ml                 Match amount of lines in response  
  -mr                 Match regexp  
  -ms                 Match HTTP response size  
  -mw                 Match amount of words in response  
  
FILTER OPTIONS:  过滤
  -fc                 Filter HTTP status codes from response. Comma separated list of codes and ranges  
  -fl                 Filter by amount of lines in response. Comma separated list of line counts and ranges  
  -fr                 Filter regexp  
  -fs                 Filter HTTP response size. Comma separated list of sizes and ranges  
  -fw                 Filter by amount of words in response. Comma separated list of word counts and ranges  
  
INPUT OPTIONS:  
  -D                  DirSearch wordlist compatibility mode. Used in conjunction with -e flag. (default: false)  
  -e                  Comma separated list of extensions. Extends FUZZ keyword.  
  -ic                 Ignore wordlist comments (default: false)  
  -input-cmd          Command producing the input. --input-num is required when using this input method. Overrides -w.  
  -input-num          Number of inputs to test. Used in conjunction with --input-cmd. (default: 100)  
  -input-shell        Shell to be used for running command  
  -mode               Multi-wordlist operation mode. Available modes: clusterbomb, pitchfork (default: clusterbomb)  
  -request            File containing the raw http request  
  -request-proto      Protocol to use along with raw request (default: https)  
  -w                  Wordlist file path and (optional) keyword separated by colon. eg. '/path/to/wordlist:KEYWORD'  
  
OUTPUT OPTIONS:  
  -debug-log          Write all of the internal logging to the specified file.  
  -o                  Write output to file  
  -od                 Directory path to store matched results to.  
  -of                 Output file format. Available formats: json, ejson, html, md, csv, ecsv (or, 'all' for all formats) (default: json)  
  -or                 Don't create the output file if we don't have results (default: false)

-H 请求头
-X http的方式，post，get...
-b cookie
-d 请求体

# 基本模糊测试

### 目录模糊测试

爆破扩展名：
```
ffuf -u http://MACHINE_IP/indexFUZZ -w /usr/share/seclists/Discovery/Web-Content/web-extensions.txt
```

爆破页面：
```
ffuf -u http://MACHINE_IP/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-words-lowercase.txt -e .php,.txt
```

爆破目录：
```
ffuf -u http://MACHINE_IP/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories-lowercase.txt
```


# 域模糊测试

### 子域模糊测试
