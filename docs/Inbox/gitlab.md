jenkins设置国内镜像



## Docker 部署/迁移

>   从  Omnibus  迁移至docker ，先要同级恢复然后停止后升级docker版本（可用资源率过低会导致主机卡死）

-   部署nginx
-   nginx 开启80，22端口转发到gitlab container上

- 部署时出现权限问题时，启动后执行

  ```shell
  docker exec -it gitlab update-permissions
  docker retart gitlab
  # 出现未touch的file touch
  # 此为共享式分布式文件的问题, https://gitlab.com/gitlab-org/omnibus-gitlab/-/blob/master/doc/settings/configuration.md#disable-storage-directories-management
  # 关闭管理权限
  
  ```

  

## Backup

>   [参考链接](https://docs.gitlab.com/ee/raketasks/backup_restore.html) 
>
>   rsync required

-   Omnibus installion `sudo gitlab-backup create`
-   Docker installion `docker exec -t <container name> gitlab-backup create`

>   Warni ng: Your gitlab.rb and gitlab-secrets.json files contain sensitive data and are not included in this backup. You will need these files to restore a backup. Please back them up manually
>
>   For Omnibus:
>
>   -   `/etc/gitlab/gitlab-secrets.json`
>   -   `/etc/gitlab/gitlab.rb`
>
>   For Docker:
>
>   	-  volumes

定时备份

```shell
0 2 * * * /opt/gitlab/bin/gitlab-backup create CRON=1 >> /var/log/gitlab/backup.log
10 2 * * * /usr/bin/rsync -ahvP --no-o --no-g /var/opt/gitlab/backups/ /mnt/backup/gitlab >> /var/log/gitlab/backup-sync.log
```

## Restore

- Omnibus

  >   ```shell
  >   chown git.git  <timestamp>.backup.tar
  >   sudo gitlab-ctl stop unicorn
  >   sudo gitlab-ctl stop puma
  >   sudo gitlab-ctl stop sidekiq
  >   sudo gitlab-ctl status
  >   
  >   sudo gitlab-backup restore BACKUP= <timestamp>
  >   ```
  >
  >   >    restore  `/etc/gitlab/gitlab-secrets.json`
  >   >
  >   >    ```shell
  >   >    sudo gitlab-ctl reconfigure
  >   >    sudo gitlab-ctl restart
  >   >    sudo gitlab-rake gitlab:check SANITIZE=true
  >   >    ```



-   Docker

    >   confirm the restore target directories are empty
    >
    >   ```shell
    >   # Stop the processes that are connected to the database
    >   docker exec -it <name of container> gitlab-ctl stop unicorn
    >   docker exec -it <name of container> gitlab-ctl stop puma
    >   docker exec -it <name of container> gitlab-ctl stop sidekiq
    >   
    >   # Verify that the processes are all down before continuing
    >   docker exec -it <name of container> gitlab-ctl status
    >   
    >   # Run the restore
    >   docker exec -it <name of container> gitlab-backup restore BACKUP= <timestamp>.backup.tar
    >   
    >   # Restart the GitLab container
    >   docker restart <name of container>
    >   
    >   # Check GitLab
    >   docker exec -it <name of container> gitlab-rake gitlab:check SANITIZE=true
    >   ```

## Update

>   -   docker  https://docs.gitlab.com/omnibus/docker/README.html#prerequisites
>   -   omnibus https://docs.gitlab.com/omnibus/update/#multi-step-upgrade-using-the-official-repositories