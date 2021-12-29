 ```shell
#!/usr/bin/env bash
# Author:   huangwx@uit.com.cn
# Notes:    ipmitool set the ipmi tool
check_ipmi() {
	echo
}

set_ipmi() {
    ipmitool lan set 1 ipaddr ${IPMI_IP}
    ipmitool lan set 1 netmask ${IPMI_MASK}
    ipmitool lan set 1 defgw ipaddr ${IPMI_GW}
    ipmitool lan set 1 access on

    ipmitool user set name 5 dev
    ipmitool user set password 5 dev
    # 设置权限
    ipmitool channel setaccess 1 5 callin=on ipmi=on link=on privilege=4
    ipmitool user enable 5
}

show_help() {
  echo
  echo "Usage: ./ipmi.sh  command ...[parameters]....
  -i                          ipmi ip
  -m                          ipmi netmask
  -g                          ipmi default gateway
  "
}

main() {
    while getopts 'i:m:g:' OPTION; do
        case "$OPTION" in
        i)
            IPMI_IP=${OPTARG}
            ;;
        m)
            IPMI_MASK=${OPTARG}
            ;;
        g)
            IPMI_GW=${OPTARG}
            ;;
        h)
            show_help
            exit
            ;;
        ?)
            show_help
        exit
            ;;
        esac
    done
    if [ "$IPMI_IP" ] && [ "$IPMI_MASK" ] && [ "$IPMI_GW" ]; then
    	set_ipmi  >/dev/null 2>&1 
    	ipmitool user test 5 16 dev
    else
        show_help
    fi
}

main "$@"

 ```



- 疑惑点，有台机器Bad Password Threshold  为Invaild时 怎么都无法访问IP

  ```shell
  Bad Password Threshold  : 3
  Invalid password disable: yes
  Attempt Count Reset Int.: 300
  User Lockout Interval   : 300
  ```

  | name            | about    | title | labels | assignees |
  | --------------- | -------- | ----- | ------ | --------- |
  | Feature Request | 功能建议 |       |        |           |

  **描述** 请具体并清楚地描述期望功能及期望的效果

  **其他说明** 其他补充内容

