## windows  vagrant + vmware

Download vagrant https://www.vagrantup.com/downloads

vmware support  https://www.vagrantup.com/docs/providers/vmware

- Vagrant VMware Utility  https://www.vagrantup.com/vmware/downloads

  启动utility服务 https://www.vagrantup.com/docs/providers/vmware/vagrant-vmware-utility

  ```cmd
  net.exe start vagrant-vmware-utility
  ```

- Vagrant VMware provider 

  ```shell
  vagrant plugin install vagrant-vmware-desktop
  ```

配置完成，直接使用vagrant up --provider=vm-...  完成