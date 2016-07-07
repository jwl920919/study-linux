# 시스템 관리 (CentOS7)


#### 폴더 및 파일 크기 순으로 Top 20

```
du -a | sort -n -r | head -n 20
```

#### 노트북 덮을 때 대기모드 진입 방지

```
vi /etc/systemd/logind.conf

#HandleLidSwitch=suspend  
==> HandleLidSwitch=ignore

#HandleLidSwitchDocked=ignore
==> HandleLidSwitchDocked=ignore
```
