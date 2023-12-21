--------------------------------------------------------------------------------

			# SERVER-STUDY #

--------------------------------------------------------------------------------

# Pre-Step 

* Windows 환경에 맞게 구성된 내용임 (Unix/Linux 설정 시 해당 환경에 맞게 설정 하면 됨 원리는 동일 함)

1.unzip server-study.zip (unzip path C:\dev)
2.JAVA_HOME = C:\dev\server-study\java\zulu8.31.0.1-jdk8.0.181-win_x64
3.add path JAVA_HOME
4.Example web Project (demo) 개발 환경 - STS 사용

--------------------------------------------------------------------------------
# Install Directory Review

C:\dev\server-study\
					Apache2
					elk
					java
					jenkins
					jenkins_home
					jmeter
					maven
					maven_repository
					scouter
					svn
					svn_repository
					tomcat1
					tomcat2
					readme.txt
					
--------------------------------------------------------------------------------
# WAS (tomcat)

	http://tomcat.apache.org/

	-[SERVER-STUDY] ADD , [SERVER-STUDY] MODIFY 주석 부분 참고
	
	1.tomcat_users.xml
	C:\dev\server-study\tomcat1\apache-tomcat-9.0.37\conf\tomcat_users.xml
	C:\dev\server-study\tomcat2\apache-tomcat-9.0.37\conf\tomcat_users.xml
	
	2.server.xml
	C:\dev\server-study\tomcat1\apache-tomcat-9.0.37\conf\server.xml
	C:\dev\server-study\tomcat2\apache-tomcat-9.0.37\conf\server.xml
	
		-Clustering/Session Replication How-To
		https://tomcat.apache.org/tomcat-9.0-doc/cluster-howto.html

	3.start (CATALINA_HOME 환경변수 설정 안한 경우 bin 디렉 토리 밑에서 실행)
	C:\dev\server-study\tomcat1\apache-tomcat-9.0.37\bin\startup.bat
	C:\dev\server-study\tomcat2\apache-tomcat-9.0.37\bin\startup.bat
					
		참고 : 톰캣 console 창에서 한글 깨짐 (https://steven-life-1991.tistory.com/91)
	
	4.review
	http://localhost:7080   tomcat1/tomcat1
	http://localhost:9080	tomcat2/tomcat2
	
	-Tomcat reference 
		http://localhost:7080/docs/
		http://localhost:7080/docs/config/
		http://localhost:7080/examples/
	
--------------------------------------------------------------------------------
# Build Tool (Maven)

	https://maven.apache.org/

	-[SERVER-STUDY] MODIFY 주석 부분 참고
	
	1.setting.xml
	C:\dev\server-study\maven\apache-maven-3.6.3\conf\settings.xml 
	
--------------------------------------------------------------------------------
# Source Version Control (svn)

	https://subversion.apache.org/
	
	svn-win32-1.7.0 - https://sourceforge.net/projects/win32svn/files/1.7.0/
	
	설정 	(1-3 단계는 압축 해제 파일에 이미 생성 되어 있음으로 skip - 4단계부터 실행)
	1.svn repository 생성 
	C:\dev\server-study\svn\bin\svnadmin create C:\dev\server-study\svn_repository

	2.사용 권한 설정
	C:\dev\server-study\svn_repository\conf\svnserve.conf 파일에 하단 내용 추가 
	[general]
	anon-access = read
	auth-access = write
	password-db = passwd

	3.사용자 생성
	C:\dev\server-study\svn_repository\conf\passwd 파일에 하단 내용 추가
	svnadmin = svnadmin

	4.서비스 등록 (cmd 창을 관리자 권한으로 실행하고 명령문 실행)
	sc create SVNserve binpath= "C:\dev\server-study\svn\bin\svnserve.exe  --service --root \"C:\dev\server-study\svn_repository\"" displayname= "Subversion Repository" depend= Tcpip start= auto

	5.프로젝트 폴더 등록
	set SVN_EDITOR=C:\Windows\notepad.exe
	C:\dev\server-study\svn\bin\svn mkdir svn://localhost/demo
	
	6.서비스 시작
	net start SVNserve

--------------------------------------------------------------------------------
# Example web Project (demo) 개발 환경 - STS 사용 
	(STS Local Host Tomcat Server , Maven 설정)
	
	https://spring.io/tools

	1.Import Getting svn demo project -> svn://localhost/demo
	
	2.Export war file
	
	3.deploy
	demo.war tomcat webapps 폴더에 복사 
	C:\dev\server-study\tomcat1\apache-tomcat-9.0.37\webapps
	C:\dev\server-study\tomcat1\apache-tomcat-9.0.37\webapps

	5.tomcat 시작 후 접속
	http://localhost:7080/demo/logincheck
	http://localhost:7080/demo/login
	http://localhost:9080/demo/logincheck
	
--------------------------------------------------------------------------------
# CI (jenkins)

	https://www.jenkins.io/

	jenkins 실행 start 파일 작성 
	C:\dev\server-study\jenkins\jenkins\start.bat
	
	start.bat 파일 내용
	-----------------------------------------------
		cd C:\dev\server-study\jenkins

		set JENKINS_HOME=C:\dev\server-study\jenkins_home

		rem C:\dev\server-study\java\zulu8.31.0.1-jdk8.0.181-win_x64\bin\java -jar jenkins.war --httpPort=9999

		java -jar jenkins.war --httpPort=9999
	-----------------------------------------------
	
	시작 후 접속
	http://localhost:9999 (admin/admin)
	
	- jenkins review
		 *jenkens 관리
			시스템 설정
			Global Tool Configuration
			플러그인 관리
			
		 *새로운 Item / 구성 
		 
		 *jenkin_repository 디렉토리 살펴보기 
			C:\dev\server-study\jenkins_home\jobs
			C:\dev\server-study\jenkins_home\workspace			

--------------------------------------------------------------------------------
# WEB Server (Apache2)

	http://httpd.apache.org/
		http://httpd.apache.org/docs/current/platform/windows.html

	-----------------------------------------------
	cmd(admin) : C:\dev\server-study\Apache2\bin

	-service name register
	httpd -k install -n "Apache2" 	
	
	-Strat Apache
	httpd -k start -n "Apache2"

	-Stop Apache
	httpd -k stop -n "Apache2"
	
	-Restart Apache
	httpd -k restart -n "Apache2"
	
	Test Config Syntax			httpd -t
	Version Details				httpd -V
	Command Line Options List	httpd -h	
	
	-----------------------------------------------		
	
	-[SERVER-STUDY] ADD , [SERVER-STUDY] MODIFY 주석 부분 참고
	 (/conf/original 설정 파일과 비교해 보면 변경 내용 쉽게 확인이 쉬움)
	
	1.httpd.conf 
	C:\dev\server-study\Apache2\conf\httpd.conf
		
		-Apache Tomcat 연동
		http://tomcat.apache.org/connectors-doc/
		https://www.lesstif.com/system-admin/apache-httpd-tomcat-connector-mod_jk-reverse-proxy-mod_proxy-12943367.html
	
		Apache Tomcat mod_jk 연계 설정 
		-workers_jk.properties , uriworkermap.properties 파일 작성
		C:\dev\server-study\Apache2\conf\workers_jk.properties
		C:\dev\server-study\Apache2\conf\uriworkermap.properties
	
		-mod_jk.so modules 폴더에 추가 
		C:\dev\server-study\Apache2\modules\mod_jk.so		
		
    2.httpd-ssl.conf
	C:\dev\server-study\Apache2\conf\extra\httpd-ssl.conf
	
	3.httpd-vhosts.conf
	C:\dev\server-study\Apache2\conf\extra\httpd-vhosts.conf		
	
		-인증서(certification)
		https://dreamlog.tistory.com/375
		https://www.securesign.kr/guides/SSL-Certificate-Convert-Format
	
--------------------------------------------------------------------------------
# Stress Test (jmeter)

	https://jmeter.apache.org/
	
	-get-started
	https://jmeter.apache.org/usermanual/get-started.html
	
	-web test plan
	https://jmeter.apache.org/usermanual/build-web-test-plan.html
	https://jmeter.apache.org/usermanual/build-adv-web-test-plan.html

	-Run
	C:\dev\server-study\jmeter\apache-jmeter-5.3\bin\jemeter.bat
	
	-Open File
	C:\dev\server-study\jmeter\sample.jmx	

--------------------------------------------------------------------------------
# APM (scouter)

	https://github.com/scouter-project/scouter	
	https://github.com/scouter-project/scouter/blob/master/scouter.document/main/Quick-Start.md
	https://github.com/scouter-project/scouter/blob/master/scouter.document/main/Setup.md

	-scouter server (port : 6180)	
	C:\dev\server-study\scouter\scouter-all-2.8.1\server\startup.bat

	-scouter host
	C:\dev\server-study\scouter\scouter-all-2.8.1\agent.host\host.bat
	
	-scouter agent (tomcat start.bat 파일에 추가)	
	C:\dev\server-study\tomcat1\apache-tomcat-9.0.37\bin\startup.bat
		set "JAVA_OPTS=%JAVA_OPTS% -javaagent:C:/dev/server-study/scouter/scouter-all-2.8.1/agent.java/scouter.agent.jar"
		set "JAVA_OPTS=%JAVA_OPTS% -Dscouter.config=C:/dev/server-study/scouter/scouter-all-2.8.1/agent.java/conf/tomcat1.conf"
		set "JAVA_OPTS=%JAVA_OPTS% -Dobj_name=tomcat1"
	
	C:\dev\server-study\tomcat2\apache-tomcat-9.0.37\bin\startup.bat
		set "JAVA_OPTS=%JAVA_OPTS% -javaagent:C:/dev/server-study/scouter/scouter-all-2.8.1/agent.java/scouter.agent.jar"
		set "JAVA_OPTS=%JAVA_OPTS% -Dscouter.config=C:/dev/server-study/scouter/scouter-all-2.8.1/agent.java/conf/tomcat2.conf"
		set "JAVA_OPTS=%JAVA_OPTS% -Dobj_name=tomcat2"

	-scouter client (port : 6100 , id/pw admin/admin)	
	C:\dev\server-study\scouter\scouter.client\scouter.exe	

--------------------------------------------------------------------------------
# Monitoring (elk)
	
	https://www.elastic.co/kr/
	https://www.elastic.co/guide/index.html
	https://www.elastic.co/guide/kr/index.html

	-ELK Stack?
	https://www.elastic.co/kr/what-is/elk-stack

	-Elasticsearch?
	https://www.elastic.co/kr/what-is/elasticsearch

	-elastic-stack
	https://www.elastic.co/kr/elastic-stack

	-elasticsearch
	https://www.elastic.co/kr/elasticsearch/

		-Elasticsearch Reference
		https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html?baymax=KR-ES-getting-started&elektra=landing-page
		https://victorydntmd.tistory.com/category/OpenSource/Elasticsearch
		
	-logstash
	https://www.elastic.co/kr/logstash
		
	-beats
	https://www.elastic.co/kr/beats

	-kibana
	https://www.elastic.co/kr/kibana

	-GrokConstructor
	http://grokconstructor.appspot.com/
	http://grokconstructor.appspot.com/do/match?example=2

	-----------------------------------------------	
	
	-elasticsearch
		C:\dev\server-study\elk\elasticsearch-7.5.0\bin\elasticsearch.bat
		http://127.0.0.1:9200/

		-index 조회
		http://localhost:9200/_cat/indices?v&pretty
			
	-logstash( 처음 설치 시 plugin update)
		logstash-plugin update logstash-input-beats
		logstash-plugin update logstash-filter-useragent
		logstash-plugin update logstash-filter-geoip
		
		-grok filter
		http://grokconstructor.appspot.com/do/match?example=2

		C:\dev\server-study\elk\logstash-7.5.0\bin\logstash.bat -f C:\dev\server-study\elk\logstash-7.5.0\config\logstash.conf
			
	-filebeat
		C:\dev\server-study\elk\filebeat-7.5.0\filebeat.exe -c C:\dev\server-study\elk\filebeat-7.5.0\filebeat.yml -e -v

	-kibana (Node 설치필요)
		C:\dev\server-study\elk\kibana-7.5.0\bin\kibana.bat
		http://127.0.0.1:5601/

