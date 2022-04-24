---
title: "Sentry Install"
date: 2022-03-30T22:34:34+08:00
categories: ["tools"]
tags: ["sentry"]
draft: true
comment: true
---

## 安裝步驟

1. 需要先把專案 clone 下來

    ```
    $ git clone git@github.com:getsentry/self-hosted.git
    ```

2. 執行前置步驟

    ```
    $ cd self-hosted
    $ ./install.sh
    
    ## 如果不需要開帳號可以用這個
    ## 不過筆者在安裝過程，使用上面的會造成 crush 不知道為何
    ## 可以先建完環境在新建使用者
    $ ./install.sh --skip-user-prompt 
    
    $ docker compose run --rm web createuser
    ```

3. 再來就等畫面出現這個就可以了

    ```
    -----------------------------------------------------------------
    
    You're all done! Run the following command to get Sentry running:
    
      docker compose up -d
    
    -----------------------------------------------------------------
    ```

4. 再來就可以執行 `docker compose` 了

    ```
    docker compose up -d
    ```


## 注意事項

- 如果遇到要清空 volume

    ```
    docker-compose down -v --remove-orphans && docker volume prune -f
    ```

- 筆者在跑 `Ensuring Relay credentials` 時遇到卡住情形

    ```
    ▶ Ensuring Relay credentials ...
    ../relay/config.yml already exists, skipped creation.
    relay Pulling 
    relay Pulled 
    Network sentry-self-hosted_default  Creating
    Network sentry-self-hosted_default  Created
    ```

    ```
    ## 不確定是不是 docker 版本問題，改成 docker-compose 就好了
    $ docker-compose --ansi never --env-file /Users/jonny/Desktop/jonny-job/self-hosted/.env run --rm --no-deps -T relay credentials generate --stdout
    ```

- 可以使用指令

    ```
    docker-compose stop   ###t停止
    
    docker-compose build   ###重新build
    
    docker-compose run --rm web upgrade  ###升级配置
    
    docker-compose up -d
    ```


## 相關連結

- [https://github.com/getsentry/self-hosted](https://github.com/getsentry/self-hosted)
- [https://develop.sentry.dev/](https://github.com/getsentry/self-hosted)
- [https://docs.sentry.io/platforms/php/guides/laravel/](https://docs.sentry.io/platforms/php/guides/laravel/)
- [https://juejin.cn/post/6844904144566910983](https://juejin.cn/post/6844904144566910983)
- [https://www.cnblogs.com/JasonLong/p/14794378.html](https://www.cnblogs.com/JasonLong/p/14794378.html)
- [https://chenng.cn/posts/Sentry前端错误监控系统搭建/](https://chenng.cn/posts/Sentry%E5%89%8D%E7%AB%AF%E9%94%99%E8%AF%AF%E7%9B%91%E6%8E%A7%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA/)
- [https://iter01.com/647717.html](https://iter01.com/647717.html)
