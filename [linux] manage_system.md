# 시스템 관리 (CentOS7)

#### 폴더 및 파일 크기 순으로

  * 폴더 및 파일 크기 순으로 Top 10
```
du -a | sort -n -r | head -n 10
```

==========================================

#### 대기 모드 진입 방지

  * 노트북 덮을 때, 대기모드 진입 방지
```
vi /etc/systemd/logind.conf

  #HandleLidSwitch=suspend  
  ==> HandleLidSwitch=ignore

  #HandleLidSwitchDocked=ignore
  ==> HandleLidSwitchDocked=ignore
```

==========================================

### WOL 설정

  * WOL 설정
```
ethtool -s eth0 wol g
```

  * WOL 확인
```
ethtool eth0 | grep Wake-on

  Wake-on: g    ==> g 확인
```

  * ifcfg에 WOL 옵션 추가
```
vi /etc/sysconfig/network-scripts/ifcfg-eth0
  
  (마지막 라인에)
  ETHTOOL_OPTS="wol g"  ==> 추가
```

==========================================
