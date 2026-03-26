Detect mimikatz và brute force với SOAR (wazuh + shuffle + thehive) 


I. Điều kiện tiên quyết
1. Soar là gì?
SOAR không phải là một công cụ đơn lẻ mà là một tập hợp các công nghệ cho phép tổ chức thu thập dữ liệu an ninh và phản ứng với các sự cố một cách tự động.
Soar gồm 3 thành phần:
●	Điều phối (Orchestration): kết nối và phối hợp nhiều công cụ bảo mật khác nhau.
●	Tự động hóa (Automation): tự động thực hiện các bước xử lý.
●	Ứng phó (Response): phản ứng nhanh với các sự cố an ninh.
2. Wazuh, shuffle, thehive là gì? 
a)	Wazuh
Wazuh là nền tảng an ninh mạng mã nguồn mở được sử dụng rộng rãi nhất, tích hợp XDR và SIEM trong một giải pháp duy nhất. Nó phân tích dữ liệu bảo mật trên các thiết bị đầu cuối, đám mây và mạng để phát hiện các mối đe dọa, ứng phó với sự cố và đảm bảo tuân thủWazuh là một nền tảng bảo mật mã nguồn mở miễn phí tích hợp các khả năng Phát hiện và Phản hồi Mở rộng (XDR) và Quản lý Thông tin và Sự kiện Bảo mật (SIEM). Nó cung cấp khả năng bảo vệ toàn diện cho các thiết bị đầu cuối và khối lượng công việc trên đám mây, cho phép các tổ chức phát hiện và ứng phó với các mối đe dọa một cách hiệu quả.
Các chức năng chính của Wazuh:
•	Phát hiện xâm nhập: Giám sát hệ thống để phát hiện các dấu hiệu truy cập trái phép hoặc hoạt động độc hại.
•	Phân tích dữ liệu nhật ký: Thu thập và phân tích nhật ký từ nhiều nguồn khác nhau để xác định các sự cố bảo mật tiềm ẩn.
•	Giám sát tính toàn vẹn tập tin: Theo dõi các thay đổi đối với các tập tin và thư mục quan trọng, cảnh báo cho quản trị viên về các sửa đổi trái phép.
•	Phát hiện lỗ hổng: Xác định các lỗ hổng đã biết trong các thành phần phần mềm và phần cứng trong mạng.
•	Đánh giá cấu hình: Đánh giá cấu hình hệ thống để đảm bảo tuân thủ các chính sách và tiêu chuẩn bảo mật.
•	Ứng phó sự cố: Cung cấp các công cụ và khả năng để ứng phó và giảm thiểu các sự cố bảo mật một cách nhanh chóng.
•	Tuân thủ quy định: Hỗ trợ các tổ chức đáp ứng các yêu cầu tuân thủ bằng cách cung cấp các biện pháp kiểm soát an ninh và công cụ báo cáo cần thiết.
•	Bảo mật đám mây: Cung cấp khả năng giám sát và bảo vệ cơ sở hạ tầng dựa trên đám mây, đảm bảo an ninh trên nhiều môi trường khác nhau.
b)	TheHive:
TheHive là một nền tảng ứng phó sự cố an ninh (SIRP) mã nguồn mở được thiết kế để hỗ trợ các Trung tâm Điều hành An ninh (SOC), Nhóm Ứng phó Sự cố An ninh Máy tính (CSIRT) và Nhóm Ứng phó Khẩn cấp Máy tính (CERT) trong việc quản lý và ứng phó hiệu quả với các sự cố an ninh. Nó cung cấp một môi trường cộng tác nơi các chuyên gia an ninh có thể phân tích, theo dõi và giải quyết các sự cố một cách hiệu quả.
Đặc điểm của TheHive
•	Quản lý hồ sơ: Tạo và quản lý các hồ sơ với thông tin chi tiết, bao gồm nhiệm vụ, các chỉ số quan sát và nhật ký.
•	Cộng tác: Nhiều người dùng có thể cùng lúc xử lý một trường hợp, thúc đẩy tinh thần làm việc nhóm và xử lý sự cố hiệu quả.
•	Tích hợp: Tích hợp liền mạch với các công cụ bảo mật khác như MISP (Nền tảng chia sẻ thông tin phần mềm độc hại) và Cortex để phân tích tự động.
•	Quản lý cảnh báo: Thu thập cảnh báo từ nhiều nguồn khác nhau, cho phép phân loại và ưu tiên nhanh chóng.
•	Bảng điều khiển tùy chỉnh: Trực quan hóa dữ liệu sự cố và theo dõi các chỉ số hiệu suất thông qua bảng điều khiển do người dùng định nghĩa.
c)	Shuffle: 
Shuffle là một nền tảng SOAR mã nguồn mở dùng để tự động hóa và điều phối các quy trình xử lý sự cố bảo mật. Nó giúp các công cụ bảo mật khác nhau kết nối với nhau và tự động thực hiện các hành động khi có cảnh báo.
Thành phần chính của Shuffle
•	Workflow: Chuỗi các bước xử lý tự động
•	Trigger / Webhook: Nơi nhận alert từ hệ thống khác
•	Apps / Integrations: Kết nối với các công cụ khác (Telegram, VirusTotal, Firewall…)
•	Playbook: Quy trình xử lý sự cố được tự động hóa
3. Để cài đặt tất cả thành 1 server thì máy cần bao nhiêu ram
Bảng dưới đây tổng hợp các yêu cầu về RAM, CPU và lưu trữ cho từng thành phần khi chạy riêng lẻ và khi kết hợp trong một máy chủ All-in-One.
Thành phần	RAM tối thiểu	RAM khuyến nghị	CPU tối thiểu
Wazuh Indexer	4 GB	8 GB	2 Cores
Wazuh Server 	2 GB	4 GB	2 Cores
Wazuh Dashboard	2 GB	4 GB	2 Cores
TheHive ( Cassandra/ES)	4 GB	8 GB	4 Cores
Shuffle 	2 GB	4 GB	2 Cores
Tổng cộng All-in-One 	12 GB	16 GB	4-8 vCPU

II. Bắt đầu
1. Mô hình mạng
 <img width="755" height="663" alt="{1AD87771-CA6C-4EDD-A6E1-46AA0BC96B96}" src="https://github.com/user-attachments/assets/d213574e-e123-4402-b29c-5553afde090b" />
Mô hình mạng được thiết kế gồm: 
•	1 máy ubuntu server cài Allin-one: Wazuh, TheHive, Shuffle có IP: 192.168.182.145
•	1 máy ubuntu victim: có IP: 192.168.182.149
•	1 máy windows victim: có IP: 192.168.182.146
•	1 máy kali làm attacker: có IP: 192.168.182.136 

2. Cài đặt
a) Cài đặt wazuh

Tải xuống trình trợ giúp cài đặt Wazuh và tệp cấu hình
curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh
curl -sO https://packages.wazuh.com/4.14/config.yml
 <img width="1350" height="99" alt="{812B0602-8136-4460-8A2A-8F139A400A71}" src="https://github.com/user-attachments/assets/d1ebb5c6-9823-4607-864f-0900d9af1fce" />
Chỉnh sửa ./config.yml thay thế tên nút và giá trị IP bằng tên và địa chỉ IP tương ứng
sudo nano config.yml
 <img width="1313" height="558" alt="{68B93701-A89B-42F4-9B9E-278794E0D18C}" src="https://github.com/user-attachments/assets/ce21dbef-cdbd-4142-9903-8c7d0d46f5db" />
Chạy trình trợ giúp cài đặt Wazuh với tùy chọn --generate-config-files tạo khóa cụm Wazuh, chứng chỉ và mật khẩu cần thiết cho quá trình cài đặt
bash wazuh-install.sh --generate-config-files
 <img width="1343" height="286" alt="{8AA91F9D-8B8C-4C97-8674-D76990DF5AC2}" src="https://github.com/user-attachments/assets/19d165cb-555f-46b4-8da5-c3cf567af318" />
Chạy trình trợ giúp cài đặt Wazuh với tùy chọn --wazuh-indexer và tên nút để cài đặt và cấu hình trình lập chỉ mục Wazuh. Tên nút phải giống với tên đã sử dụng trong config.yml cấu hình ban đầu, ví dụ: node-1
bash wazuh-install.sh --wazuh-indexer node-1
 <img width="1326" height="484" alt="{BD63FD4A-02F2-46BA-A894-E04D88EF90F9}" src="https://github.com/user-attachments/assets/237426ba-7da3-4cf5-aae4-ed892aba1874" />
Chạy trình trợ giúp cài đặt Wazuh với tùy chọn --start-clustertrên bất kỳ nút lập chỉ mục Wazuh nào để tải thông tin chứng chỉ mới và khởi động cụm
bash wazuh-install.sh --start-cluster
 <img width="1311" height="259" alt="{FB894CF3-0329-4F12-902E-CE41F7EC77D5}" src="https://github.com/user-attachments/assets/453a69cb-ebc0-4151-98fd-b5fdeed1e8da" />
Chạy lệnh sau để lấy mật khẩu quản trị:
tar -axf wazuh-install-files.tar wazuh-install-files/wazuh-passwords.txt -O | grep -P "\'admin\'" -A 1
 <img width="1338" height="64" alt="{CD9C7680-7909-4FFA-8386-146619797323}" src="https://github.com/user-attachments/assets/6d1cda77-fb5d-4392-89fa-1b4940d132ab" />
Chạy lệnh sau để xác nhận quá trình cài đặt thành công. Thay thế bằng địa chỉ IP của trình lập chỉ mục Wazuh và sử dụng mật khẩu nhận được từ kết quả của lệnh trước đó
curl -k -u admin:QCPuEqo.MlAEvPo3PSIaQ997iUTpg+zV https://192.168.182.145:9200
Chạy lệnh sau để kiểm tra xem cụm máy chủ có hoạt động chính xác hay không
curl -k -u admin:QCPuEqo.MlAEvPo3PSIaQ997iUTpg+zV https://192.168.182.145:9200/_cat/nodes?v
<img width="1321" height="393" alt="{07771991-C448-49A7-A512-DAB0184724F0}" src="https://github.com/user-attachments/assets/eb4583aa-374a-4e17-bf2d-6862dae056ba" />
Chạy trình trợ giúp cài đặt Wazuh với tùy chọn --wazuh-server theo sau là tên nút để cài đặt máy chủ Wazuh. Tên nút phải giống với tên được sử dụng trong config.yml cho cấu hình ban đầu, ví dụ: wazuh-1
bash wazuh-install.sh --wazuh-server wazuh-1
 <img width="1346" height="493" alt="{BF57D3EE-AF5F-4283-8EB1-9A1EFEECF9BA}" src="https://github.com/user-attachments/assets/348ff02b-de42-4b6e-a2cf-c9e8f2a58f9d" />
Chạy trình trợ giúp cài đặt Wazuh với tùy chọn --wazuh-dashboard và tên nút để cài đặt và cấu hình bảng điều khiển Wazuh. Tên nút phải giống với tên đã sử dụng trong config.yml quá trình cấu hình ban đầu, ví dụ dashboard
bash wazuh-install.sh --wazuh-dashboard dashboard
 <img width="1298" height="623" alt="{1536CA86-FE85-4465-9114-6A0F47536143}" src="https://github.com/user-attachments/assets/cd088832-ea49-44ac-938c-daebc63595ce" />
Chạy câu lệnh sau để lấy tất cả mật khẩu mà trình trợ giúp cài đặt Wazuh đã tạo trong wazuh-passwords.txt tệp bên trong wazuh-install-files.tar tệp lưu trữ
tar -O -xvf wazuh-install-files.tar wazuh-install-files/wazuh-passwords.txt
 <img width="1047" height="751" alt="{56AD5318-39F1-4928-8CC7-A5AA2514DF43}" src="https://github.com/user-attachments/assets/8b1294dd-d7cf-4589-8aaa-1e96fb614f06" />
Hãy đăng nhập web https://192.168.182.145:443 với tài khoản admin và mật khẩu QCPuEqo.MlAEvPo3PSIaQ997iUTpg+zV
 <img width="1052" height="560" alt="{C0F168A5-84ED-4703-B6AC-9F8248AA533E}" src="https://github.com/user-attachments/assets/fb99789b-b53f-48a7-ab72-9a1c018aa223" />
Như này là được.
b)	Cài đặt thehive
Cài đặt các gói và thư viện:
apt install wget gnupg apt-transport-https git ca-certificates ca-certificates-java curl software-properties-common python3-pip lsb-release
 <img width="1037" height="589" alt="{27CB4B5C-D9D8-4956-A256-3C2D04D7EBF0}" src="https://github.com/user-attachments/assets/44d28539-569a-4828-980b-672d6055ef3b" />
Cài đặt java
wget -qO- https://apt.corretto.aws/corretto.key | sudo gpg --dearmor -o /usr/share/keyrings/corretto.gpg
echo "deb [signed-by=/usr/share/keyrings/corretto.gpg] https://apt.corretto.aws stable main" | sudo tee -a /etc/apt/sources.list.d/corretto.sources.list
sudo apt update
sudo apt install java-common java-11-amazon-corretto-jdk
echo JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto" | sudo tee -a /etc/environment
export JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto"
 <img width="1068" height="582" alt="{89D5C269-5FEF-4BB6-9557-8438D90D7C3D}" src="https://github.com/user-attachments/assets/d1e330f5-ff71-47f7-96cd-569ccb6615a2" />
Kiểm tra phiên bản Java
 <img width="1142" height="171" alt="{F35767D9-0449-422A-A07D-BFDDD322B8FE}" src="https://github.com/user-attachments/assets/c2e931a5-0f64-44dc-b1d4-55d6ba29a566" />
Cài đặt Cassandra
wget -qO -  https://downloads.apache.org/cassandra/KEYS | sudo gpg --dearmor  -o /usr/share/keyrings/cassandra-archive.gpg
echo "deb [signed-by=/usr/share/keyrings/cassandra-archive.gpg] https://debian.cassandra.apache.org 41x main" |  sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
sudo apt update
sudo apt install cassandra
 

Cài đặt elasticsearch
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch |  sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
sudo apt-get install apt-transport-https
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" |  sudo tee /etc/apt/sources.list.d/elastic-7.x.list
sudo apt update
sudo apt install elasticsearch
 

Cài đặt TheHive
wget -O /tmp/thehive_5.5.7-1_all.deb https://thehive.download.strangebee.com/5.5/deb/thehive_5.5.7-1_all.deb
wget -O /tmp/thehive_5.5.7-1_all.deb.sha256 https://thehive.download.strangebee.com/5.5/sha256/thehive_5.5.7-1_all.deb.sha256
wget -O /tmp/thehive_5.5.7-1_all.deb.asc https://thehive.download.strangebee.com/5.5/asc/thehive_5.5.7-1_all.deb.asc
 

Cài đặt package.
sudo apt-get install /tmp/thehive_5.5.7-1_all.deb

 

Cấu hình tệp cassandra: sudo nano /etc/cassandra/cassandra.yaml
 
 
 
 

Sau khi lưu, chạy lênh: 
systemctl stop cassandra
rm -rf /var/lib/cassandra/*
sudo systemctl start cassandra
sudo systemctl enable cassandra
 

Sau khi cấu hình cassandra thì chúng ta cấu hình elasticsearch:
sudo nano /etc/elasticsearch/elasticsearch.yml

 

Vì wazuh đã chiếm port 9200 nên bắt buộc ở đây mình phải chuyển sang 9201, không thì khi start sẽ bị lỗi

 

sudo systemctl start elasticsearch
sudo systemctl enable elasticsearch

Sửa quyền của đường dẫn /opt/thp/
 

Cấu hình tệp application.conf : sudo nano /etc/thehive/application.conf
 
 
sudo systemctl start thehive
sudo systemctl enable thehive

Sau khi làm xong sẽ truy cập đường dashboard TheHive: http://192.168.182.145:9000
 

Đăng nhập với user: admin@thehive.local
                                      pass: secret

 

Vào được này là xong nha

c)	Cài đặt shuffle
sudo apt update && sudo apt install -y docker.io docker-compose
sudo systemctl enable docker
sudo systemctl start docker
git clone https://github.com/Shuffle/Shuffle.git
cd Shuffle
 

sudo chown -R 1000:1000 shuffle-database
sudo swapoff -a
sudo docker-compose up -d
sudo docker restart shuffle-opensearch

Ở đây bạn cũng phải vào tệp .yml để sửa port thành 9202 vì các port khác đã bị chiếm rồi
 

Sau khi cài xong thì bạn nhớ restart lại như trong hình sao cho nó done hết cả 4 là vào web
http://192.168.182.145:3001 hoặc https://192.168.182.145:3443 ( shuffle chạy cả 2 được )
 

Mới vào sẽ phải đợi 1 xíu cho nó xong hết rồi mới được điền user và pass ( điền này là đăng ký luôn )

d)	Cài đặt wazuh agent cho ubuntu và windows 
Đã xong tất cả giờ đến bước cài đặt agent cho cả 2 hệ điều hành 
Đối với windows : hãy mở powershell và chạy 2 lệnh dưới:
Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.14.3-1.msi -OutFile $env:tmp\wazuh-agent; msiexec.exe /i $env:tmp\wazuh-agent /q WAZUH_MANAGER='192.168.182.145'

NET START Wazuh
 
Đối với ubuntu chạy 2 lệnh sau: 
wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.14.3-1_amd64.deb && sudo WAZUH_MANAGER='192.168.182.145' dpkg -i ./wazuh-agent_4.14.3-1_amd64.deb
 
sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
 

III. Cấu hình để soar phát hiện mimikatz và brute force
1. Cấu hình để phát hiện mimikatz
a) Để phát hiện mimikatz trên windows, chúng ta cần cài sysmon
Chúng ta sẽ bắt đầu bằng cách cài đặt Sysmon từ liên kết sau: Tải xuống Sysmon
Chúng ta cũng cần tải xuống tệp cấu hình Sysmon từ URL sau: Cấu hình mô-đun Sysmon .
 
Tiếp theo,giải nén thư mục nén Sysmon và đặt tệp cấu hình Sysmon vào cùng thư mục với Sysmon.
 
Tiếp theo, mở PowerShell với quyền quản trị để cấu hình Sysmon.
.\Sysmon64.exe -i .\sysmonconfig.xml
 
Kiểm tra xem Sysmon đã được cài đặt trên hệ thống chưa.
Get-Service Sysmon64
 
Chúng ta sẽ thêm trong tệp ossec.conf trên windows: 

 

Sau đó restart: 
Restart-Service WazuhSvc

b) Sau khi đã cài xong Sysmon
Chúng ta sẽ phải sửa file ossec.conf: 
sudo nano /var/ossec/etc/ossec.conf
 
Xong hãy restart wazuh-manager: sudo systemctl restart wazuh-manager

Tiếp theo hãy cấu hình filebeat: 
sudo nano /etc/filebeat/filebeat.yml
 
Rồi restart filebeat: sudo systemctl restart filebeat
Tiếp theo, chúng ta cần tạo chỉ mục cho kho lưu trữ. Hãy tiến hành bước này.
 
 
 
 
 

Sau khi khởi tạo xong thì có thể vào lại discovery để xem
 

Để detect được mimikatz chúng ta cần thêm rule vào: sudo nano /var/ossec/etc/rules/local_rules.xml

 
Sau đó restart: sudo systemctl restart wazuh-manager



2. Cấu hình để phát hiện brute force
Đầu tiên mở file: sudo nano /var/ossec/etc/rules/local_rules.xml
Và dán dòng sau: 
  <rule id="5764" level="10" frequency="3" timeframe="60">
    <if_matched_sid>5710</if_matched_sid>
    <same_source_ip />
    <description>Multiple SSH login attempts using non-existent usernames.</description>
    <mitre>
      <id>T1110</id>
    </mitre>
  </rule>

 

Tiếp theo hãy mở tệp : sudo nano /var/ossec/etc/ossec.conf và dán 
  <integration>
    <name>shuffle</name>   
<hook_url>your url</hook_url>
    <rule_id>100002</rule_id>
    <alert_format>json</alert_format>
  </integration>


 

Vẫn trong file đó, tìm đến phần active response
 

Chỉnh sửa thành: 

<command>firewall-drop</command>
<location>local</location>
<rules_id>5764</rules_id>                                          
<timeout>no</timeout>

 

Sau đó restart: sudo systemctl restart wazuh-manager

IV. Kiểm thử và kết quả
1. Mô hình kịch bản
 
1.	Ứng dụng Wazuh Agent sẽ gửi các sự kiện đến Wazuh Manager.
2.	Quản lý Wazuh sẽ nhận được các sự kiện.
3.	Wazuh Manager sẽ tạo một cảnh báo và sau đó gửi nó đến Shuffle.
4.	Sau khi nhận được cảnh báo, Shuffle sẽ tiến hành thu thập thông tin từ VirusTotal và VirusTotal sẽ gửi kết quả trở lại cho Shuffle.
5.	Shuffle sẽ gửi thông báo đến TheHive.
6.	Shuffle cùng lúc sẽ gửi email qua Internet cho Chuyên viên phân tích SOC.
7.	Khi chuyên viên phân tích SOC nhận được email, email đó cần chứa thông tin chi tiết về cảnh báo và có thể đặt câu hỏi cho chuyên viên phân tích SOC: Bạn có muốn thực hiện hành động cụ thể nào không? (Có/Không)
8.	Tùy thuộc vào phản hồi nhận được, hành động đó sau đó sẽ được gửi đến Shuffle thông qua Internet.
9.	Shuffle sẽ chuyển tiếp Hành động Phản hồi đến Wazuh Manager.
10.     Wazuh manager sẽ gửi lệnh xuống cho agent để buộc agent chặn hoặc xoá.
2. Kiểm thử và kết quả
a) Triển khai công cụ Mimikatz 
Đầu tiên, để triển khai Mimikatz, chúng ta sẽ mở máy victim lên, sau đó mở powershell.
 
Chạy mã độc mimikatz
.\mimikatz.exe
 
Sau khi chạy xong mã độc mimikatz, chúng ta sẽ sang wazuh check
 
 
 
Sau khi đã check wazuh xong, chúng ta sang shuffle check
 
 
 
 
Vào theHive check xem có nhận được thông báo hay không
 
 
Sau khi đã triển khai kiểm thử mimikatz, chúng ta sẽ thấy cảnh báo được gửi về telegram cho quản trị viên
 

b) Triển khai tấn công brute force
Đầu tiên, để triển khai tấn công brute force, chúng ta sẽ ping thử từ máy kali đến máy victim
Ping 192.168.182.149
 
Khi check xong, chúng ta sẽ chạy lệnh sau trên máy kali để tấn công brute force
hydra -L users.txt -P passwords.txt ssh://192.168.182.149
 
Sau khi đã chạy lệnh tấn công brute force, chúng ta sẽ vào check trên shuffle, theHive, telegram xem có nhận cảnh báo không
 
 
 
 
 
 
Check trong theHive
 
 
 
Check trong telegram

 
Từ cảnh báo ở telegram, gửi lệnh chặn IP tấn công
http://192.168.182.145:3001[copy lệnh bôi đen là chặn]
Sau khi đã chặn IP tấn công, ping thử lại từ máy kali đến máy victim thì thấy không ping được nữa.
 

V. Thảo luận 
1. Đánh giá hiệu quả của quy trình tự động hóa (SOAR)
Việc triển khai mô hình kết hợp giữa Wazuh, Shuffle và TheHive đã chứng minh được khả năng tối ưu hóa quy trình phản ứng sự cố. Thay vì quy trình thủ công truyền thống nơi quản trị viên phải kiểm tra log trên Wazuh, tạo case trên một nền tảng khác và thực hiện chặn IP bằng tay hệ thống hiện tại đã tạo ra một workflow khép kín.
Kết quả thực nghiệm với các cuộc tấn công thử nghiệm từ Kali Linux (như Brute force hay sử dụng Mimikatz) cho thấy thời gian từ khi phát hiện đến khi phản ứng (MTTR - Mean Time To Respond) đã giảm xuống đáng kể, từ vài phút xuống còn tính bằng giây. Việc nhận cảnh báo trực tiếp qua Telegram giúp đội ngũ kỹ thuật có thể giám sát hệ thống ở bất cứ đâu, đảm bảo tính sẵn sàng cao.
2. Tính thực tiễn đối với các doanh nghiệp
Một trong những điểm đáng thảo luận nhất của hệ thống này là tính kinh tế. Bằng việc sử dụng hoàn toàn các giải pháp mã nguồn mở, doanh nghiệp có thể xây dựng một trung tâm điều hành an ninh (SOC) chuyên nghiệp mà không phải chịu chi phí bản quyền đắt đỏ như các giải pháp thương mại.
Mô hình All-in-One chạy trên Ubuntu là một lựa chọn hợp lý cho các SMBs có hạ tầng máy chủ hạn chế. Tuy nhiên, qua quá trình vận hành, cần lưu ý rằng khi số lượng Agent tăng lên, tài nguyên hệ thống (đặc biệt là RAM và CPU) sẽ bị chiếm dụng đáng kể. Do đó, doanh nghiệp cần có kế hoạch nâng cấp phần cứng hoặc tách biệt các dịch vụ khi quy mô mạng mở rộng.
3. Phân tích các hạn chế và rủi ro
Mặc dù hệ thống hoạt động ổn định, vẫn còn một số tồn tại cần được xem xét kỹ lưỡng:
Điểm yếu duy nhất (Single Point of Failure): Việc tập trung tất cả các công cụ (Wazuh Manager, TheHive, Shuffle) trên một máy chủ duy nhất tiềm ẩn rủi ro lớn. Nếu máy chủ này bị tấn công hoặc gặp sự cố phần cứng, toàn bộ hệ thống giám sát và phản ứng sẽ bị tê liệt.
Tỷ lệ cảnh báo giả (False Positives): Các quy tắc (rules) cấu hình trong Wazuh cần được tinh chỉnh liên tục. Ví dụ, việc tự động chặn IP sau 3 lần nhập sai mật khẩu có thể gây gián đoạn công việc cho nhân viên hợp lệ nếu họ vô tình quên mật khẩu.
4. Hướng phát triển và nâng cấp
Để hệ thống trở nên hoàn thiện hơn, các hướng phát triển tiếp theo nên tập trung vào:
●	Ứng dụng Machine Learning: Thay vì chỉ dựa vào các quy tắc tĩnh (static rules), việc tích hợp các mô hình học máy để phát hiện hành vi bất thường sẽ giúp hệ thống đối phó tốt hơn với các cuộc tấn công chưa có chữ ký .
●	Triển khai cụm: Chuyển đổi mô hình sang dạng Cluster để đảm bảo tính sẵn sàng cao và khả năng cân bằng tải khi số lượng thiết bị giám sát tăng lên.

 
