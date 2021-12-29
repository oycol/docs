### Linux  脚本收集

- OPENWRT获取程序PID / 启动进程  / 

    ```shell
    #!/bin/bash /etc/rc.common
    #Create by oycol<oycol527@outlook.com>
    EXTRA_COMMANDS="status"
    EXTRA_HELP="        status  	Check service is running"
    START=99
    
    LOG_FILE="/tmp/netclient.logs"
    
    start() {
      if [ ! -f "${LOG_FILE}" ];then
          touch "${LOG_FILE}"
      fi
      local PIDS=($(ps -ef|grep "netclient checkin -n all"|grep -v grep|awk '{print $1}'))
      if [ ${PIDS} ];then
        echo "service is running"
        return
      fi 
      bash -c "while [ 1 ]; do /etc/netclient/netclient checkin -n all >> ${LOG_FILE} 2>&1;sleep 15;\
               if [ $(ls -l ${LOG_FILE}|awk '{print $5}') -gt 10240000 ];then tar zcf "${LOG_FILE}.tar" -C / "tmp/netclient.logs"  && > ${LOG_FILE};fi;done &"
      echo "start"
    }
     
    stop() {
      local PIDS=($(ps -ef|grep "netclient checkin -n all"|grep -v grep|awk '{print $1}'))
      for i in "${PIDS[@]}"; do
        kill $i
      done 
      echo "stop"
    }
    
    status() {
      local PIDS=($(ps -ef|grep "netclient checkin -n all"|grep -v grep|awk '{print $1}'))
      if [ ${PIDS} ];then
        echo -e "netclient[${PIDS}] is running \n"
      else
        echo -e "netclient is not running \n" 
      fi
    }
    
    ```
    
- sh edtion

    ```shell
    #!/bin/sh /etc/rc.common
    #Create by oycol<oycol527@outlook.com>
    EXTRA_COMMANDS="status"
    EXTRA_HELP="        status      Check service is running"
    START=99
    
    LOG_FILE="/tmp/netclient.logs"
    
    start() {
      if [ ! -f "${LOG_FILE}" ];then
          touch "${LOG_FILE}"
      fi
      local PID=$(ps -ef|grep "netclient checkin -n all"|grep -v grep|awk '{print $1}')
      if [ "${PID}" ];then
        echo "service is running"
        return
      fi
      bash -c "while [ 1 ]; do /etc/netclient/netclient checkin -n all >> ${LOG_FILE} 2>&1;sleep 15;\
               if [ $(ls -l ${LOG_FILE}|awk '{print $5}') -gt 10240000 ];then tar zcf "${LOG_FILE}.tar" -C / "tmp/netclient.logs"  && > $LOG_FILE;fi;done &"
      echo "start"
    }
    
    stop() {
      local PID=$(ps -ef|grep "netclient checkin -n all"|grep -v grep|awk '{print $1}')
      if [ "${PID}" ];then
        kill "${PID}"
      fi
      echo "stop"
    }
    
    status() {   
      local PID=$(ps -ef|grep "netclient checkin -n all"|grep -v grep|awk '{print $1}')
      if [ "${PID}" ];then
        echo -e "netclient[${PID}] is running \n"
      else                                     
        echo -e "netclient is not running \n"  
      fi                                       
    }
    ```
    
- 判断文件大小

     ```shell
     FILE="PATH"
     if [ -e $FILE ]; then
       LOG_FILE_SIZE=`ls -l $LOG_FILE|awk '{print $5}'`
       LOG_FILE_SIZE=`stat --format=%s $FILE`
       if [ $LOG_FILE_SIZE -gt 10240000 ];then 
         tar $FILE
       fi
     fi
     ```

​    

