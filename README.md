__OSS Final Project__
=======================================================
학번: 21600567
이름: 이하림
---------------

## 라즈베리파이에 FTP 서버를 구축하여 NAS를 만들 것입니다.

* 라즈베리파이에 vsftpd라는 FTP서버 프로그램을 설치합니다.

	sudo apt-get install vsftpd

* 그 다음 vsftpd의 설정파일로 들어가서 몇몇 설정을 수정한 뒤에 저장합니다.

	sudo nano /etc/vsftpd.conf

	다음과 같은 명령어의 주석을 삭제합니다.

	> local_enable=YES

	> write_enable=YES

	> local_umask=022

	> chroot_local_user=YES

	> chroot_list_enable=YES

	> chroot_list_file=/etc/vsftpd.chroot_list

* user list file을 만들어 라즈베리파이의 아이디를 추가합니다.

	sudo nano /etc/vsftpd.chroot_list

	pi를 입력하고 저장합니다.

* vsftpd를 restart합니다.

	sudo systemctl restart vsftpd

* 마지막으로 라즈베리파이를 부팅할 때마다 FTP서버를 자동실행할 수 있도록 설정합니다.

	sudo systemctl enable vsftpd


* 그러면 라즈베리파이의 IP주소를 이용하여 FTP서버의 접속이 가능합니다.

	*ftp://라즈베리파이IP주소*


* 여기서 한가지 문제점이 있습니다. 라즈베리파이를 연결을 새롭게 할 때마다 IP주소가 변하여 매번 새로운 IP주소를 가지고 FTP서버의 접속하기에는 많은 번거로움이 있습니다. 그래서 이번에는 라즈베리파이의 주소를 일정한 IP주소로 할당할 수 있도록 고정할 것입니다.


	1. 명령어 ifconfig를 통해 IP주소를 확인합니다.
	2. 명령어 netstat -nr을 통해 고정 아이피로 변경합니다.
	3. sudo nano /etc/dhcpcd.conf를 입력하여 아래의 해당하는 문구를 찾아 주석을 풀고 수정합니다.

		interface eth0

		static ip_address="원하는 IP주소 입력"

		static routers="공유기IP"

		static domain_name_servers=8.8.8.8

	4. 마지막으로 sudo reboot를 입력하여 라즈베리파이를 재부팅합니다.



이제 FTP서버를 같은 와이파이(local)가 연결되어 있는 기기들끼리 사용할 수 있게 되었습니다.
