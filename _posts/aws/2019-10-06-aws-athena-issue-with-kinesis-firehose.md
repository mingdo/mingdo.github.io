---
layout: post
title:  "AWS Athena Issue With Kinesis Firehose"
date:   2019-10-06 19:28:00
author: Mingdo
categories: aws
---

AWS Athena 서비스를 통해 Kinesis Firehose S3 백업 데이터를 가공하던 중 발생한 이슈에 대한 정리

# 상황
- API 서버에서 Kinesis Firehose 로 JSON 데이터 전송
- 데이터는 ElasticSearch 와 S3 에 저장
    - Kinesis Firehose 백업 기능을 사용해 S3 에 저장
- S3 백업 데이터를 가공해 DB 에 저장해야함
    - 매일 Lambda Function 이 실행돼 전날에 대한 통계를 DB 에 저장 

# 문제해결
### Kinesis Firehose backup 기능에서는 S3 데이터 prefix 변경이 제한적이기 때문에 Athena 자동 파티셔닝 명령을 사용할 수 없음
- year=2019/month=10/day=04 와 같은 형식은 자동 파티셔닝 가능하지만 현재 2019/10/04 와 같은 형식으로 저장돼 자동 파티셔닝 불가능
    - Kinesis Firehose backup 기능에서는 2019 앞에 year 와 같은 prefix 를 붙일 수 없음
- 수동 명령으로 1일 1회 파티셔닝 진행
```
ALTER TABLE elb_logs_raw_native_part ADD PARTITION (year='2019',month='10',day='04') location 's3://athena-examples/elb/plaintext/2019/10/04/'
```

### JSON 데터 간 구분자가 없어 Athena 에서 첫 번째 JSON 객체만 읽음
- ```{'a':'b'}{'c':'d'}``` 와 같이 객체간 구분자가 없음
- API 단에서 JSON 전송 시 개행 추가한 뒤 재배포
- 기존 S3 데이터 임시 인스턴스에 sink ``` aws s3 sync <S3 버킷 경로> <저장 경로> ```
- JSON 객체간 개행 추가 스크립트 실행
    ``` 
    for file in $(find <파일경로> -type f)
    do 
            sed -i -E "s/\}\{/\}\n\{/g" $file
    done
    ```
- 변경한 파일 S3 에 sink ``` aws s3 sync <저장 경로> <S3 버킷 경로> ```

### S3 에 저장된 파일의 시간 관련 데이터는 KST 기준인데 S3 폴더 및 파일 구조는 UTC 기준으로 저장되어 있어 일 별 조회 시 데이터 누락됨
- 10월 5일 데이터는 2019/10/04 폴더에 있어야 하는데 폴더 구조가 UTC 기준으로 되어 있어 오전 9시까지의 데이터가 2019/10/03 폴더에 존재  
- 10월 5일의 데이터가 필요하면 10/03 와 10/04 폴더를 모두 조회한 뒤 파일 내부의 시간 데이터로 10월 4일 데이터 조회
  ```
  // Athena query
  // 10/03, 10/04 파티셔닝 되어 있어야 함
  select * 
  from sample_table 
  where 
  (
    (
      year = year(timestamp '2019-10-05 01:00:00' - interval '1' day)
      AND month = month(timestamp '2019-10-05 01:00:00' - interval '1' day)
      AND day = day(timestamp '2019-10-05 01:00:00' - interval '1' day)
    )
    OR 
    (
      year = year(timestamp '2019-10-05 01:00:00' - interval '2' day)
      AND month = month(timestamp '2019-10-05 01:00:00' - interval '2' day)
      AND day = day(timestamp '2019-10-05 01:00:00' - interval '2' day)
    )
  )
  and date(actionoccurredat) = date(timestamp '2019-10-05 01:00:00' - interval '1' day)
  ```
