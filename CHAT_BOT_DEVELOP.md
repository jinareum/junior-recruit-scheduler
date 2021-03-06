# AWS S3 & TravisCI & Telegram 챗봇 구현 가이드


```yaml
language: node_js
node_js:
  - "6"

branches:
  only:
    - master

script: "npm run build"

before_deploy:
  - npm install
  - mkdir -p deploy
  - mv db.json deploy/db.json

deploy:
  - provider: s3
    access_key_id: $AWS_ACCESS_KEY # declared in Travis repo settings
    secret_access_key: $AWS_SECRET_KEY
    bucket: junior-recruit-scheduler
    region: ap-northeast-2
    skip_cleanup: true
    local_dir: deploy
    acl: public_read
    wait-until-deployed: true
    on:
      repo: jojoldu/junior-recruit-scheduler
      branch: master

after_deploy:
  - echo "주니어 개발자 채용 정보 배포 진행중입니다."

notifications:
  webhooks: https://fathomless-fjord-24024.herokuapp.com/notify

```

전체 공지 기능이 별도로 없어서 사용자 ID를 저장한 후에 일괄 전송해야합니다.

[참고](https://github.com/atipugin/telegram-bot-ruby/issues/130)

* [AWS Lambda + API Gateway + DynamoDB + node.js 사용기 (삽질기)](https://medium.com/@yumenohosi/aws-lambda-api-gateway-dynamodb-node-js-%EC%82%AC%EC%9A%A9%EA%B8%B0-%EC%82%BD%EC%A7%88%EA%B8%B0-b5352e00b396)