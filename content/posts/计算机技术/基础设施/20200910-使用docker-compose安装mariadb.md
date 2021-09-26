---
title: 使用docker-compose安装mariadb
author: Spaceack
date: 2020-09-10 21:00:00
update: 2020-09-10 21:00:00
categories: ["基础设施"]
tags: 
  - Docker
  - docker-compose
  - mariadb
---

- step1: 创建目录

  ```bash
  mkdir -p /server/mariadb/{config,data,log}
  chmod -R 777 /server/mariadb
  ```

- step2: 编写docker-compose.yml

  ```yaml
  version: '3.1'
  services:
    mariadb:
      image: mariadb:10.5.5
      container_name: "mariadb1"
      restart: always
      environment:
        MYSQL_USER: "root"
        MYSQL_PASSWORD: "123456"
        MYSQL_ROOT_PASSWORD: "123456"
        TZ: "Asia/Shanghai"
      ports:
        - "3306:3306"
      volumes:
        - ./data:/var/lib/mysql
        - ./log:/var/log/mysql
        - ./conf/my.cnf:/etc/mysql/my.cnf
  ```

- step3: 拷贝/etc/mysql/my.cnf 到 /server/mariadb/conf/my.cnf

    ```bash
    docker exec -it mariadb1 bash
    ```

  此时目录树：

      - server/
      - mariadb/
          - data/
          - log/
          - config/
              - my.cnf
          - docker-compose.yml

  ```conf
  # The MariaDB configuration file
  #
  # The MariaDB/MySQL tools read configuration files in the following order:
  # 0. "/etc/mysql/my.cnf" symlinks to this file, reason why all the rest is read.
  # 1. "/etc/mysql/mariadb.cnf" (this file) to set global defaults,
  # 2. "/etc/mysql/conf.d/*.cnf" to set global options.
  # 3. "/etc/mysql/mariadb.conf.d/*.cnf" to set MariaDB-only options.
  # 4. "~/.my.cnf" to set user-specific options.
  #
  # If the same option is defined multiple times, the last one will apply.
  #
  # One can use all long options that the program supports.
  # Run program with --help to get a list of available options and with
  # --print-defaults to see which it would actually understand and use.
  #
  # If you are new to MariaDB, check out https://mariadb.com/kb/en/basic-mariadb-articles/

  #
  # This group is read both by the client and the server
  # use it for options that affect everything
  #
  [client-server]
  # Port or socket location where to connect
  # port = 3306
  socket = /run/mysqld/mysqld.sock

  # Import all .cnf files from configuration directory
  !includedir /etc/mysql/conf.d/
  !includedir /etc/mysql/mariadb.conf.d/

  ```

- step4: 运行启动

    ```bash
    docker-compose up -d
    ``` 