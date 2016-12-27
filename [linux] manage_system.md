[Back](https://github.com/songagi/study-linux/blob/master/README.md)

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
 ...
 Wake-on: g    ==> g 확인
```

  * ifcfg에 WOL 옵션 추가
```
vi /etc/sysconfig/network-scripts/ifcfg-eth0
  
  ...
  ETHTOOL_OPTS="wol g"  ==> 마지막 라인에 추가
```

==========================================

### 접속 로그 분석

 * 로그인 성공한 IP 보
 
```
cat /var/log/secure* | grep Accepted | awk '{print $9"\t"$11"\t"$14}' | sort | uniq

 ...
 root    121.166.104.62  ssh2
 songagi 211.63.144.199  ssh2
```

 * IP grep
 
```
cat /var/log/secure* | grep Accepted
```

 * IP Grep (일부 IP 제외)

```
cat /var/log/secure* | grep Accepted | egrep -v "(121.166.104.62|211.63.144.199)"
```

 * 침입 시도 IP 확인

```
cat /var/log/fail2ban.log* | grep "] Ban" | awk '{print $NF}' | sort | uniq -c | sort -n
```
 
 * 접속 기록 확인(last)
 
```
last
```

 * 특정 계정 접속 확인
```
last userid
```

 * 특정 시각 이전 접속 확인
 
 ```
 last -t 20161212040000
 ```
 
==========================================

[Back](https://github.com/songagi/study-linux/blob/master/README.md)
