Linux Essential 과정 정리

제 01장.LinuxCommand
제 02장 Directory Administration

Linux 관리자 과정
* Linux 기초 관리자 과정
* Linux 서버 관리자 과정
* Linux 서비스/보안 관리자 과정


복제된 운영체제에서 변경 사항

* 호스트 이름
	# hostnamectl
	# hostnamectl set-hostname server1.example.com
* IP 설정
	# nm-connection-editor
	#nmcli connection up <porfile>





-
-------------------------------------------------------------------------------------

* TUI (Text User Interface), CLI (Command Line Interface)
* GUI (Graphic User Interface)

CentOS6 이하
동작수준/동작레벨 (Runlevel)

CentOS7 이상 
Runlevel -> Targert 으로 바뀜


언어 변경  방법

LANG 변수 - 언어 지정 변수
* ko_KR.UTF-8 : 한국어
* en_US.UTF-8 : 영어


리눅스 선수지식
* Runlevel == Target
	runlevel 3 == muti-user.target
	runlevel 5 == graphical.target
	# systemctl isolate muti-user.target | graphical.target
	#systemctl set-default multi-user.target | graphical.target
* 서비스 기동
	# systemctl enable | disable firewalld
	# systemctl start | stop firewalld
* 제어문자 Control Charater

	<CTRL + C>, <CTRL + D>
* 도움말과 암호변경
	man CMD
		# man ls
		#man -k calendar
		#man -f passwd
		#man -s 5 passwd

		[참고] 명령어 옵션 확인
		#ls -help
		# man ls
	passwd CMD
		# passwd fedora

###########################################
제 04장 리눅스 기본 정보 확인
###########################################

시스템 기본 정보확인
	uname CMD
		# uname -a 커널, cpu
		# cat /etc/redhat-release 배포판 버전 
		[참고] 사이트 redhat documentation
	date CMD
	     # date 08301300
	     # date +%m%d
	
	     [실무예] NTP 서버에 시간 동기화
	     # (vi)gedit /etc/chrony.conf
	     server time.bora.net iburst
	     # systemctl stop chronyd
                 # systemctl start chronyd
	cal CMD

###########################################
제 05장 파일 및 디렉토리 관리
###########################################

디렉토리 이동 관련 명령어
	[참고] 파일시스템 기본 구조
	# man 7 hier
	
	pwd CMD
		[참고]  PS1 변수 ($HOME/ .bash)
		# export PS1=`[\u@\h \w]\$ `
	cd CMD
		경로(Path)
		* 상대경로(Relative Path) # cd dir1
		* 절대경로(Absolut Path) # cd /dir1
		
		[참고]  자신의 홈디렉토리 이동
		# cd
		# cd ~
   		# cd $HOME

		[참고]  지정된 사용자 홈디렉토리 이동
		# cd ~fedora

		[실무예]  이전 디렉토리 이동하기
		# cd -

		[실무예] 옆에 있는 디렉토리 이동하기
		# cd ../dir2
디렉토리 관리 명령어
	ls CMD
	     # ls -l dir1
	     # ls -ld dir1
                 OPTIONS: -l, -d, -R, -a, -i, -h, -t, -r
	[참고] alias
	(선언) # alias ls='ls -l | grep "^-"'
	(확인) # alias
	(해제) # unalias ls
	
	[실무예] 실무에서 많이 사용되는 ls CMD
	# cd /Log_dir
	# ls -altr

	mkdir CMD
		# mkdir -p dir1/dir2 (있으면 그대로 없으면 생성)
	rmdir CMD
	(주의) 파일/디렉토리 삭제 주의해야 한다.
	# rm -rf dir1

파일 관리 명령어
	touch CMD
		# touch -t 08301300 file1
	cp CMD
		# cp file1 file2
		# cp file1 dir1
		# cp -r dir1 dir2
		OPTION: -r, -i, -f, -p, -a

		[실무예]  설정 파일을 백업하는 경우
		# cp -p httpd.conf httpd.conf.orig
		# cp -a /src /src.orig
		[실무예]
		#cp /dev/null file.log
		#cat /dev/null > file.log
		# > file.log


	파일스시템: 파일을 저장하고 관리하는 구조 체계
	ex) ext4, xfs, fat32, ntfs
	mv CMD
		# mv file1 file2
		# mv file1 dir1
		# mv dir1 dir2
		OPTION: -i, -f

	[참고 ] 와일드 캐릭터 
	*  ?  { }  [ ] 

	rm CMD
		# rm -rf dir1
		
		[실무예] rm 명령어로 지운 파일 복원하기
		(TUI) extundelete CMD
		(GUI) TestDisk 툴
		


파일 내용 확인 명령어
	cat CMD
		# cat -n file1
		# cat file1 file2 > file3
	more CMD
		# CMD | more
		# ps -ef | more
		# cat /etc/services | more
		# netstat -an | more
		# systemctl list-unit-files
	head  CMD
		[실무예]
		# alias pps='ps -ef | head -1 ; ps -ef | grep $1'
		# alias nstate='netstat -antup | head -2 ; netstat -antup | grep $1'
	tail CMD
		# top
		# tail -f /var/lo/messages
	
		# tail -f /var/log/messages | egrep -i 'warn|error|fail|crit|alert|emerg'
		# tail -f /var/log/messages /var/log/secure

	[참고] telnet 서비스 기동하기
	# yum install telnet telnet-server
	# systemctl enable --now telnet.socket(enable과 start 시켜준다)
	


기타 관리용 명령어
	wc CMD
	[참고] 데이터 수집
	# ps -ef | tail -n +2 | wc -l
	# cat /etc/passwd | wc -l
	# rpm -qa | wc -l
	#df -k / | tail -1 | awk '{print $5}'
	# cat /var/log/messages | grep 'Jan 19' | grep 'Started Telnet Server' | wc -l

	
	su CMD
	# su root
	# su - root

	sudo CMD (/etc/sudoers, /etc/sudoers.d/*) 임시적으로 관리자권한
	# sudo CMD
	# sudo -l
	# sudo -i
	
	id CMD 옵션 중요하지 않음
	
	groups CMD
	id, groups 명령어만
	
	last CMD (/var/log/wtmp)
		# last -i
		# last -f /var/log/wtmp.20230128
	lastlog (/var/log/lastlog)

	lastb CMD (/var/log/btmp)
	
	who CMD (/var/run/utmp)

	w CMD
		[참고] 모니터링 구문
		while true
		do
		echo "----'date'------"
		CMD
		sleep 2
		done 

		[참고] watch CMD
	
	exit CMD 명령어만 알면됨

###########################################
제 06장 파일 종류
###########################################	

파일 종류
* 일반 파일(Regular File)

* 디렉토리 파일(Directory File)

* 링크 파일(Link File)
	*하드 링크 파일(Hard Link)
		# ln file1 file2
	* 심볼릭 링크 파일(Symbolic Link File)
		# ln -s file1 file2

* 장치 파일(Device)
	* 블록 장치 파일(Block Device File)
	* 캐릭터 장치 파일(Character Device File)
#######################
제 07장 파일 속성 관리
########################

chown CMD
	# chown -R fedora:fedora /home/fedora
chgrp CMD (명령어만)

chmod CMD
	퍼미션 변경
	*심볼릭 모드(Symbolic Mode) # chmod u+x file1
	*옥탈 모드 (Octal Mode) #chmod 755 file1
	파일 & 디렉토리 퍼미션 의미
	* 파일 (r  / w  /  x)
	* 디렉토리 (r (ls -l CMD)  / w (생성/삭제)  /  x (cd CMD))
	디렉토리는 x권한이있어야한다
	
	퍼미션 적용 순서
	* UID check -> GIDs check -> other
	umask CMD (002 -> 022 -> 027)
	* (관리자) /etc/bahsrc
	* (사용자) $HOME/ .bashrc
MAC
IP address ? 호스트 구분하는 번호
Port address ? 서비스 구분하는 번호
* 0 ~ 65535
* 0 ~ 1023 : 잘 알려진 서비스를 위해 할당하는 포트 번호
	22: SSH
	23: Telnet
	25: SMTP
	53: DNS
	80: HTTP
	110: POP3
	143: IMAP4
	123: NTP
	...
[참고] 자격증
* AWS SSA
* 정보 처리 기사
+
* 리눅스 마스터 2급 OS
* 네트워크 관리사 2급 Network

--------------------------------------------------------------------
1. 집 PC에 환경 세팅하기
* NAS 이미지
* VMnet8 > 192.168.10.0/24 /255.255.255.0
* nm-connection-editor
	# nmcli connection up <profile>
2. 학원 PC 사용해야 하는 경우
* ChromeRemoteDeskTop
* anydesk
* teamview

[참고] 노트북 설정
Notebook(WIFI) <---> AP

Notebook
VMware > Edit > Virtual Network Editor
	* VMnet0: 무선 NIC 선택
	* VMnet8: 192.168.10.0/24
--------------------------------------------------------------------------
#################################
제 08장 VI/VIM 편집기
################################

VI 편집기 모드
* 명령 모드 (Command/edit mode)
* 입력 모드 (Insert/Input Mode)
* 최하위행 모드 (Last Line/Ex Mode)

VI 편집기 환경파일
* $HOME/ .vimrc
	set nu
	set ai
	set ts=4

##################################
제 09장 사용자와 통신할 때 사용하는 명령어
##################################

mail/mainx CMD
	# mailx -s '[  ok   ] server1' admin@example.com < /root/report.txt
wall CMD
	# wall < /etc/MESS/work.txt
	
	[참고] 긴급 작업 절차
	# touch /etc/nologin
	# wall < /etc/MESS/work.txt
	...
	# fuser -cu /home
	# fuser -ck /home
	작업 진행
	# rm -f /etc/nologin

####################################
제 10장 관리자가 알아두면 유용한 명령어
####################################
cmp/diff CMD
	[실무예] 설정 파일 비교
	# diff httpd.conf httpd.conf.OLD
	
	[실무예] 디렉토리 마이그레이션 종료 후 비교
	# diff -r /was1 /was2
sort CMD
	# CMD | sort -k 3
	# CMD | sort -k 3 -r
file CMD


##########################################
제 15장 원격접속과 파일 전송
##########################################


파일 전속
	scp
	sftp
원격 접속

	ssh
	# ssh server2
	# ssh server2 CMD
	
	[참고] Public Key Authentication
	# ssh-keygen
	# ssh-copy -i id_rsa.pub root@sever2 (192.168.10.20)

