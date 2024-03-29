<!--
title: 'NginX Log to JSON'
description: 'This is a function that converts input records from NginX access log format to JSON.'
layout: Doc
framework: v1
platform: AWS
language: Python
authorLink: 'https://github.com/kihoair'
authorName: 'Kota Kaihori'
-->
# kinesis-firehose-nginx-log-to-json

These functions convert the access log and error log from Nginx to JSON so that Elasticsearch can use the log sent by Kinesis Firehose.

## Runtime
Python 3.7

## Usage

### access log

sample nginx access log: 

```log
127.0.0.1 - - [02/Jan/2013:23:46:57 +0900] "GET / HTTP/1.1" 200 177 "-" "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:17.0) Gecko/20100101 Firefox/17.0" "-"
127.0.0.1 - - [02/Jan/2013:23:46:58 +0900] "GET /favicon.ico HTTP/1.1" 200 318 "-" "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:17.0) Gecko/20100101 Firefox/17.0" "-"
```

test event:

```json
{
  "invocationId": "fir",
  "region": "ap-northeast-1",
  "records": [
    {
      "recordId": "49546986683135544286507457936321625675700192471156785154",
      "approximateArrivalTimestamp": 1495072949453,
      "data": "MTI3LjAuMC4xIC0gLSBbMDIvSmFuLzIwMTM6MjM6NDY6NTcgKzA5MDBdICJHRVQgLyBIVFRQLzEuMSIgMjAwIDE3NyAiLSIgIk1vemlsbGEvNS4wIChYMTE7IFVidW50dTsgTGludXggeDg2XzY0OyBydjoxNy4wKSBHZWNrby8yMDEwMDEwMSBGaXJlZm94LzE3LjAiICItIg=="
    },
    {
      "recordId": "49546986683135544286507457936321625675700192471156785155",
      "approximateArrivalTimestamp": 1495072949456,
      "data": "MTI3LjAuMC4xIC0gLSBbMDIvSmFuLzIwMTM6MjM6NDY6NTggKzA5MDBdICJHRVQgL2Zhdmljb24uaWNvIEhUVFAvMS4xIiAyMDAgMzE4ICItIiAiTW96aWxsYS81LjAgKFgxMTsgVWJ1bnR1OyBMaW51eCB4ODZfNjQ7IHJ2OjE3LjApIEdlY2tvLzIwMTAwMTAxIEZpcmVmb3gvMTcuMCIgIi0i"
    }
  ]
}
```

The expected result should be similar to:

```bash
START RequestId: c325ceef-36d9-46ad-a15b-e508e1333719 Version: $LATEST
49546986683135544286507457936321625675700192471156785154
49546986683135544286507457936321625675700192471156785155
Processing completed.  Successful records 2, Failed records 0.
END RequestId: c325ceef-36d9-46ad-a15b-e508e1333719
REPORT RequestId: c325ceef-36d9-46ad-a15b-e508e1333719	Duration: 99.47 ms	Billed Duration: 100 ms	Memory Size: 128 MB	Max Memory Used: 56 MB	Init Duration: 114.61 ms	
```

### error log

sample nginx error log:

```log
2019/10/03 16:40:39 [error] 8724#8724: *3745908 access forbidden by rule
```

test event:

```
{
  "invocationId": "fir",
  "region": "ap-northeast-1",
  "records": [
    {
      "recordId": "49546986683135544286507457936321625675700192471156785154",
      "approximateArrivalTimestamp": 1495072949453,
      "data": "MjAxOS8xMC8wMyAxNjo0MDozOSBbZXJyb3JdIDg3MjQjODcyNDogKjM3NDU5MDggYWNjZXNzIGZvcmJpZGRlbiBieSBydWxlCg=="
    }
  ]
}
```

The expected result should be similar to:

```bash
START RequestId: 4fb63188-a52b-41e1-b201-04727668f8e8 Version: $LATEST
49546986683135544286507457936321625675700192471156785154
Processing completed.  Successful records 1, Failed records 0.
END RequestId: 4fb63188-a52b-41e1-b201-04727668f8e8
REPORT RequestId: 4fb63188-a52b-41e1-b201-04727668f8e8	Duration: 71.88 ms	Billed Duration: 100 ms	Memory Size: 128 MB	Max Memory Used: 55 MB	Init Duration: 112.60 ms	
```
