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

------------------------------------------------------
| Wazuh Indexer           | 4 GB  | 8 GB  | 2 Cores  |
------------------------------------------------------
| Wazuh Server            | 2 GB  |	4 GB  |	2 Cores  |
------------------------------------------------------
| Wazuh Dashboard         |	2 GB  |	4 GB  |	2 Cores  |
------------------------------------------------------
| TheHive ( Cassandra/ES) |	4 GB  |	8 GB  |	4 Cores  |
------------------------------------------------------
| Shuffle                 |	2 GB  |	4 GB  |	2 Cores  |
------------------------------------------------------
| Tổng cộng All-in-One    | 12 GB |	16 GB |	4-8 vCPU |
------------------------------------------------------

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

<img width="1427" height="662" alt="{BA886FB3-608F-453B-B5AA-7EBD0E7FC76B}" src="https://github.com/user-attachments/assets/0f1a4ad8-a925-4a97-890c-1c9b6d5bcb2b" />

Cài đặt elasticsearch

wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch |  sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg

sudo apt-get install apt-transport-https

echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" |  sudo tee /etc/apt/sources.list.d/elastic-7.x.list

sudo apt update

sudo apt install elasticsearch

<img width="1425" height="662" alt="{B699B645-C97E-4BDC-A6A5-A3C05CEAA516}" src="https://github.com/user-attachments/assets/7d76cf76-1728-4255-9082-a825db018000" />

Cài đặt TheHive

wget -O /tmp/thehive_5.5.7-1_all.deb https://thehive.download.strangebee.com/5.5/deb/thehive_5.5.7-1_all.deb

wget -O /tmp/thehive_5.5.7-1_all.deb.sha256 https://thehive.download.strangebee.com/5.5/sha256/thehive_5.5.7-1_all.deb.sha256

wget -O /tmp/thehive_5.5.7-1_all.deb.asc https://thehive.download.strangebee.com/5.5/asc/thehive_5.5.7-1_all.deb.asc
 
<img width="1425" height="598" alt="{12646300-C4D7-499C-A882-2441AB46BA37}" src="https://github.com/user-attachments/assets/a40593c6-4f28-44d6-9c3d-e446c2d7f116" />

Cài đặt package.

sudo apt-get install /tmp/thehive_5.5.7-1_all.deb

 <img width="1348" height="639" alt="{AC4085D3-34B7-4A47-B487-74B86BFC3C92}" src="https://github.com/user-attachments/assets/4fe9826e-0342-48d1-b57f-8b4a719290ab" />

Cấu hình tệp cassandra: sudo nano /etc/cassandra/cassandra.yaml
 
 <img width="1019" height="667" alt="{45973E26-76A1-4006-A4E0-46AAA97D83AB}" src="https://github.com/user-attachments/assets/bf4cb76a-ff30-42dc-a265-be7f0256a808" />

 <img width="1002" height="732" alt="{D24D4590-9978-4AB3-97CA-FBBD9C56F8A7}" src="https://github.com/user-attachments/assets/6ec16395-a63e-45de-bfc5-73fb8f512811" />

 <img width="881" height="469" alt="{F1639089-897F-43F0-BCE5-2C30788ECD53}" src="https://github.com/user-attachments/assets/b9eecd96-9f4f-460d-89e0-a93c6ef97088" />

<img width="840" height="689" alt="{A553DC1C-2231-4CB3-8D14-86A36ED6CCFF}" src="https://github.com/user-attachments/assets/ef599748-707c-4891-9a74-508fe4cecc24" />

Sau khi lưu, chạy lênh: 

systemctl stop cassandra

rm -rf /var/lib/cassandra/*

sudo systemctl start cassandra

sudo systemctl enable cassandra
 
<img width="1481" height="353" alt="{325D1597-C02D-4268-A020-427DE7CBC3AA}" src="https://github.com/user-attachments/assets/fec94151-a1c0-4338-b9bd-74c08ce3c014" />

Sau khi cấu hình cassandra thì chúng ta cấu hình elasticsearch:

sudo nano /etc/elasticsearch/elasticsearch.yml

<img width="1412" height="685" alt="{536AA001-41BD-49E3-BE48-98A60991A6E8}" src="https://github.com/user-attachments/assets/851e3c45-cd5f-45f9-9809-72ad723d63fb" />

Vì wazuh đã chiếm port 9200 nên bắt buộc ở đây mình phải chuyển sang 9201, không thì khi start sẽ bị lỗi

<img width="1377" height="695" alt="{90687412-3F0C-426E-97D6-DAC32451BF34}" src="https://github.com/user-attachments/assets/8e396b2b-4b43-4fa2-a7ec-d2d2ccc23d74" />

sudo systemctl start elasticsearch

sudo systemctl enable elasticsearch

Sửa quyền của đường dẫn /opt/thp/
 
<img width="1270" height="661" alt="{72F4205E-432B-4DBD-950D-58158554EC3C}" src="https://github.com/user-attachments/assets/de4e9830-d420-4835-82dc-2913ec3ace3c" />

Cấu hình tệp application.conf : sudo nano /etc/thehive/application.conf
 
 <img width="1342" height="654" alt="{FC83850F-8BB2-4C14-A135-6DAA2731F52B}" src="https://github.com/user-attachments/assets/8eabf552-fe55-401f-8911-1bc186d10927" />

<img width="1364" height="668" alt="{D7C5AFD6-7276-41B6-9C6D-B1D48BC12929}" src="https://github.com/user-attachments/assets/4eb403b8-c4e1-4c5b-8288-c99a19b3b4b5" />

sudo systemctl start thehive

sudo systemctl enable thehive

Sau khi làm xong sẽ truy cập đường dashboard TheHive: http://192.168.182.145:9000
 
<img width="1275" height="712" alt="{6E39FE32-E690-4378-88DD-E8D1EC44FB71}" src="https://github.com/user-attachments/assets/9978e6ee-b876-46ca-ba9c-cf56f80c4958" />

Đăng nhập với user: admin@thehive.local
              pass: secret

<img width="1277" height="709" alt="{A5490564-3F78-4A86-900D-665BA3498191}" src="https://github.com/user-attachments/assets/11dcb49d-1a3d-462b-8389-87c3875ff7c0" />
 
Login được là xong.

c)	Cài đặt shuffle

sudo apt update && sudo apt install -y docker.io docker-compose

sudo systemctl enable docker

sudo systemctl start docker

git clone https://github.com/Shuffle/Shuffle.git

cd Shuffle
 
<img width="1253" height="261" alt="{19327F74-0F04-4BC8-8D6C-84706947CE53}" src="https://github.com/user-attachments/assets/fd7e3127-7c77-47ff-bc97-35a012242737" />

sudo chown -R 1000:1000 shuffle-database

sudo swapoff -a

sudo docker-compose up -d

sudo docker restart shuffle-opensearch

Ở đây bạn cũng phải vào tệp .yml để sửa port thành 9202 vì các port khác đã bị chiếm rồi
 
<img width="1277" height="614" alt="{6F6579C3-7A0B-42ED-85AB-45AD831D5A56}" src="https://github.com/user-attachments/assets/823a3916-4276-464f-985a-f6b14c60902e" />

Sau khi cài xong thì bạn nhớ restart lại như trong hình sao cho nó done hết cả 4 là vào web
http://192.168.182.145:3001 hoặc https://192.168.182.145:3443 ( shuffle chạy cả 2 được )
 
<img width="1277" height="619" alt="{79F0AE90-6514-4694-B80A-A6D80903517F}" src="https://github.com/user-attachments/assets/e65ba498-f866-428a-8759-b1faec6407a3" />

Mới vào sẽ phải đợi 1 xíu cho nó xong hết rồi mới được điền user và pass (điền này là đăng ký luôn)

d)	Cài đặt wazuh agent cho ubuntu và windows 

Đã xong tất cả giờ đến bước cài đặt agent cho cả 2 hệ điều hành 

Đối với windows : hãy mở powershell và chạy 2 lệnh dưới:

Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.14.3-1.msi -OutFile $env:tmp\wazuh-agent; msiexec.exe /i $env:tmp\wazuh-agent /q WAZUH_MANAGER='192.168.182.145'

NET START Wazuh

 <img width="1276" height="217" alt="{76853BAC-FA49-4B3F-8ABE-A20868BC0DB7}" src="https://github.com/user-attachments/assets/1049946a-b187-4cef-86b0-f138abf48dae" />

Đối với ubuntu chạy 2 lệnh sau: 

wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.14.3-1_amd64.deb && sudo WAZUH_MANAGER='192.168.182.145' dpkg -i ./wazuh-agent_4.14.3-1_amd64.deb

 <img width="1277" height="455" alt="{60EA51B0-ADD3-4CEC-A334-9128E90C8761}" src="https://github.com/user-attachments/assets/fd596ec5-a83b-49fe-8397-f80f550e0803" />

sudo systemctl daemon-reload

sudo systemctl enable wazuh-agent

sudo systemctl start wazuh-agent
 
<img width="1278" height="717" alt="{3FD7B945-B1C3-4B54-B0C9-39A5B1B68311}" src="https://github.com/user-attachments/assets/0d7e2b83-2602-4f01-93b3-1b633a80c2bd" />

III. Cấu hình để soar phát hiện mimikatz và brute force

1. Cấu hình để phát hiện mimikatz

a) Để phát hiện mimikatz trên windows, chúng ta cần cài sysmon

Chúng ta sẽ bắt đầu bằng cách cài đặt Sysmon từ liên kết sau: Tải xuống Sysmon

Chúng ta cũng cần tải xuống tệp cấu hình Sysmon từ URL sau: Cấu hình mô-đun Sysmon .

 <img width="1282" height="417" alt="{0BD537DD-1366-4DB7-B8F2-2AB758303A95}" src="https://github.com/user-attachments/assets/80c2595d-72ec-4196-8752-68bbe39384c3" />

Tiếp theo,giải nén thư mục nén Sysmon và đặt tệp cấu hình Sysmon vào cùng thư mục với Sysmon.

 <img width="1277" height="203" alt="{2CF99954-C11E-4CFE-8DA4-66121CEC7521}" src="https://github.com/user-attachments/assets/0a42cb06-29e4-4d03-8ae9-b0df0e78d722" />

Tiếp theo, mở PowerShell với quyền quản trị để cấu hình Sysmon.

.\Sysmon64.exe -i .\sysmonconfig.xml

 <img width="1240" height="615" alt="{ECE3A613-2DE7-429D-88E0-BA98B5AF74CE}" src="https://github.com/user-attachments/assets/7c1a43b9-a9fb-432e-9a0c-c6c24b30a35c" />

Kiểm tra xem Sysmon đã được cài đặt trên hệ thống chưa.

Get-Service Sysmon64

 <img width="963" height="182" alt="{C1E9613A-3A1A-4E2A-A4ED-E95D17E31E96}" src="https://github.com/user-attachments/assets/798177ba-a765-414c-8972-a5699bea614f" />

Chúng ta sẽ thêm trong tệp ossec.conf trên windows: 

 <img width="1061" height="696" alt="{6CF7DCD7-1158-4130-B05D-CF2D8EB0037C}" src="https://github.com/user-attachments/assets/3cbf1acf-3696-4a00-a1af-052bd9dfcfa9" />

Sau đó restart: 

Restart-Service WazuhSvc

b) Sau khi đã cài xong Sysmon

Chúng ta sẽ phải sửa file ossec.conf: 

sudo nano /var/ossec/etc/ossec.conf

 <img width="963" height="620" alt="{BD7A4C4C-FE93-4C44-B39A-96F1C5D58E61}" src="https://github.com/user-attachments/assets/c2aae104-ed12-40c1-8b07-ff95a3a6415f" />

Xong hãy restart wazuh-manager: sudo systemctl restart wazuh-manager

Tiếp theo hãy cấu hình filebeat: 

sudo nano /etc/filebeat/filebeat.yml

 <img width="1019" height="637" alt="{56841585-3F09-4E32-A3F3-B5C0B966AEB9}" src="https://github.com/user-attachments/assets/f07d4ed4-d11d-48c4-af37-b7fae8d98e79" />

Rồi restart filebeat: sudo systemctl restart filebeat

Tiếp theo, chúng ta cần tạo chỉ mục cho kho lưu trữ. Hãy tiến hành bước này.
 
<img width="1066" height="659" alt="{741965EF-37E8-4EEB-B87A-EC504902FA92}" src="https://github.com/user-attachments/assets/c6f2d896-bc89-4d53-b451-123caf8fc9ac" />
 
 <img width="1046" height="656" alt="{97B77C53-DCA8-49F5-AC4B-0BC0F0DD8D28}" src="https://github.com/user-attachments/assets/c092405f-95e2-439f-bab6-29536737cf4b" />

<img width="1053" height="639" alt="{070D711A-2246-4287-BB57-A76714FD8083}" src="https://github.com/user-attachments/assets/f46f81de-e890-43a5-8f7f-5698a51ec559" />
 
 <img width="1055" height="607" alt="{8C4ACEB0-BAF4-4A0F-870E-9573D23BA45B}" src="https://github.com/user-attachments/assets/886433c4-eba7-4301-8a98-bd2996bf75b8" />

<img width="1055" height="626" alt="{D5D9B54E-3049-42CA-B854-054F48F758B1}" src="https://github.com/user-attachments/assets/867ad9a3-7664-4d56-95a2-403838cd51dd" />

Sau khi khởi tạo xong thì có thể vào lại discovery để xem
 
<img width="1059" height="160" alt="{0751FE78-CCD8-41B0-8809-8B8E10FF3B6E}" src="https://github.com/user-attachments/assets/466d1438-79e1-4f40-b43d-45dce1e321f9" />

Để detect được mimikatz chúng ta cần thêm rule vào: sudo nano /var/ossec/etc/rules/local_rules.xml

<img width="1251" height="660" alt="{A98736ED-3FB1-47B9-B816-CE11D0496858}" src="https://github.com/user-attachments/assets/435e2918-918a-4442-9a31-f04da73c4aa0" />
 
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

<img width="1159" height="657" alt="{408BF417-1494-4FAA-A146-297EC3A91088}" src="https://github.com/user-attachments/assets/b06beb6b-20b0-4c3b-b48d-d411e2f1ff09" />

Tiếp theo hãy mở tệp : sudo nano /var/ossec/etc/ossec.conf và dán 
  
  <integration>
    <name>shuffle</name>   
<hook_url>your url</hook_url>
    <rule_id>100002</rule_id>
    <alert_format>json</alert_format>
  </integration>

<img width="1349" height="706" alt="{E35D1F53-DB19-4A69-899B-AF3BD47E5582}" src="https://github.com/user-attachments/assets/d8e3c9c4-1792-4898-aff0-836fb51e8abb" />

Vẫn trong file đó, tìm đến phần active response
 
<img width="1278" height="757" alt="{54E3C9F2-F4A2-4264-B1FF-C1AE3F5E4058}" src="https://github.com/user-attachments/assets/6d6528c8-7f0f-49ae-b5d1-ae5899a38d61" />

Chỉnh sửa thành: 

<command>firewall-drop</command>
<location>local</location>
<rules_id>5764</rules_id>                                          
<timeout>no</timeout>

 <img width="1278" height="601" alt="{3FB6D46E-0009-46DF-8253-1D38124766C7}" src="https://github.com/user-attachments/assets/0758a058-f39d-439c-a905-ec645e626161" />

Sau đó restart: sudo systemctl restart wazuh-manager

IV. Kiểm thử và kết quả

1. Mô hình kịch bản

 <img width="1197" height="749" alt="{183269E0-78E2-4A38-AEF5-71A08FE050B1}" src="https://github.com/user-attachments/assets/fa59cfc5-5565-40bd-9b52-be951bbde91a" />

B1 Ứng dụng Wazuh Agent sẽ gửi các sự kiện đến Wazuh Manager.

B2.	Quản lý Wazuh sẽ nhận được các sự kiện.

B3.	Wazuh Manager sẽ tạo một cảnh báo và sau đó gửi nó đến Shuffle.

B4.	Sau khi nhận được cảnh báo, Shuffle sẽ tiến hành thu thập thông tin từ VirusTotal và VirusTotal sẽ gửi kết quả trở lại cho Shuffle.

B5.	Shuffle sẽ gửi thông báo đến TheHive.

B6.	Shuffle cùng lúc sẽ gửi email qua Internet cho Chuyên viên phân tích SOC.

B7.	Khi chuyên viên phân tích SOC nhận được email, email đó cần chứa thông tin chi tiết về cảnh báo và có thể đặt câu hỏi cho chuyên viên phân tích SOC: Bạn có muốn thực hiện hành động cụ thể nào không? (Có/Không)

B8.	Tùy thuộc vào phản hồi nhận được, hành động đó sau đó sẽ được gửi đến Shuffle thông qua Internet.

B9.	Shuffle sẽ chuyển tiếp Hành động Phản hồi đến Wazuh Manager.

B10. Wazuh manager sẽ gửi lệnh xuống cho agent để buộc agent chặn hoặc xoá.

2. Kiểm thử và kết quả

a) Triển khai công cụ Mimikatz 

Đầu tiên, để triển khai Mimikatz, chúng ta sẽ mở máy victim lên, sau đó mở powershell.

 <img width="1280" height="713" alt="{E23E0EBA-8C0B-494C-BE91-814DB2179F4E}" src="https://github.com/user-attachments/assets/1dd4afbd-69d8-4dbe-915d-49375cae4af3" />

Chạy mã độc mimikatz
.\mimikatz.exe

 <img width="1281" height="701" alt="{EEAF8B06-5ABC-4008-B97C-4EFC63A199A0}" src="https://github.com/user-attachments/assets/a271c5da-d74f-4502-8fc5-4652ef56e909" />

Sau khi chạy xong mã độc mimikatz, chúng ta sẽ sang wazuh check
 
<img width="1277" height="710" alt="{E96F0467-070A-4E10-B4E4-56D16DC5C3C2}" src="https://github.com/user-attachments/assets/1fb36c0e-3e09-47fd-86d1-07c7c902c66b" />
 
 <img width="1278" height="703" alt="{BB74E1FA-1CD3-48D4-A543-1D3FE8198610}" src="https://github.com/user-attachments/assets/9d4ab34d-4406-4286-a7e8-5f8295b6365d" />

<img width="1278" height="716" alt="{4F3BBA91-3997-4DFF-8FE7-B4A5D2434E20}" src="https://github.com/user-attachments/assets/8219f9c4-552f-4047-ba50-644c983de80a" />

Sau khi đã check wazuh xong, chúng ta sang shuffle check
 
 <img width="1279" height="710" alt="{382B7A8D-277E-4A32-91C2-8904D8F176F0}" src="https://github.com/user-attachments/assets/d4bf4fb5-31a8-48fb-b4db-33beb82a6dfa" />

 <img width="1275" height="717" alt="{9A1BF9C1-1DC3-449E-8772-F5E694197E9F}" src="https://github.com/user-attachments/assets/e625164d-690c-46cd-8f46-d549076f6d0a" />

<img width="1278" height="697" alt="{9399A188-5315-4DD4-AC8E-8134FCAA43F2}" src="https://github.com/user-attachments/assets/7ab8fed9-2a69-41b9-9b12-ab53fa34c872" />

 <img width="1279" height="714" alt="{C45271E3-7B69-4995-B830-EA8C993D937F}" src="https://github.com/user-attachments/assets/edcbb9c6-94b0-498f-afaf-9ee71a023dd4" />

Vào theHive check xem có nhận được thông báo hay không
 
 <img width="1279" height="712" alt="{C6A4EF4C-D23C-41F3-B6D9-AF20E156BFC0}" src="https://github.com/user-attachments/assets/136b89e0-9717-47f8-8b6a-e1e6540a6903" />

<img width="1281" height="705" alt="{B5BC5EF3-537B-464B-B613-B529E5423595}" src="https://github.com/user-attachments/assets/9f2b94cb-ae55-46e0-9a22-dfcee8af4546" />

Sau khi đã triển khai kiểm thử mimikatz, chúng ta sẽ thấy cảnh báo được gửi về telegram cho quản trị viên
 
<img width="1279" height="697" alt="{CC5158DA-9F3E-4221-8E5D-F8203470BA8F}" src="https://github.com/user-attachments/assets/e5c78f59-58f4-4302-831a-c029c73ea2a5" />

b) Triển khai tấn công brute force

Đầu tiên, để triển khai tấn công brute force, chúng ta sẽ ping thử từ máy kali đến máy victim

Ping 192.168.182.149

 <img width="1280" height="712" alt="{EBAAEB60-9C23-46EE-BD75-94F24A1AB625}" src="https://github.com/user-attachments/assets/8ead186a-b948-4d3e-bca9-6f31db115cdb" />

Khi check xong, chúng ta sẽ chạy lệnh sau trên máy kali để tấn công brute force

hydra -L users.txt -P passwords.txt ssh://192.168.182.149

 <img width="1279" height="711" alt="{E754F6E9-6FAF-4B67-A693-4092C61D2CD4}" src="https://github.com/user-attachments/assets/af4a11ad-6aba-4c82-9ad7-f50fb288ff64" />

Sau khi đã chạy lệnh tấn công brute force, chúng ta sẽ vào check trên shuffle, theHive, telegram xem có nhận cảnh báo không
 
<img width="1279" height="715" alt="{2A26FEB6-C10F-4CBD-A04D-2EF444AD3213}" src="https://github.com/user-attachments/assets/4fa2d586-0ded-4c65-895e-15968ac7df5d" />
 
<img width="1277" height="708" alt="{E5144EC3-84E1-4F45-8D4A-7BCD7231AD1E}" src="https://github.com/user-attachments/assets/16a22822-66e8-4a11-b97b-265bad1ab0bf" />
 
 <img width="1279" height="715" alt="{92849966-F913-40FC-94B4-915DF4C0A91E}" src="https://github.com/user-attachments/assets/3d67306a-31ec-4bdb-b51f-956fb6a6934a" />

 <img width="1280" height="712" alt="{E195B907-8731-428C-9EBC-F4BC9E155C5E}" src="https://github.com/user-attachments/assets/934cb728-8800-4e71-ba83-ea7c6ebc360d" />

 <img width="1278" height="708" alt="{CAB41FD7-C34E-4B78-B5D5-82F98FD265C4}" src="https://github.com/user-attachments/assets/27195369-30fc-4695-91f6-31a7395aac3d" />

<img width="1280" height="713" alt="{FBD357E4-F11C-44C3-B77D-C87A7E2FB4CC}" src="https://github.com/user-attachments/assets/ca964964-3562-441b-a536-a52eaa3b422f" />

Check trong theHive
 
 <img width="1279" height="713" alt="{E5A3FCE5-B16E-4A66-9213-AE056BA5F922}" src="https://github.com/user-attachments/assets/f00bbf6d-8000-4ec0-a85d-53d62815597b" />

<img width="1279" height="713" alt="{14C8F698-0EFB-4CF5-B57A-3866F41FE3BF}" src="https://github.com/user-attachments/assets/c5f09378-e3b5-4ca7-8f5a-14d2898b53d0" />

 <img width="1278" height="701" alt="{E7F824D1-A4F6-41A2-8212-06F4EEB41A2B}" src="https://github.com/user-attachments/assets/573f6fe4-b851-4692-b13e-8b93004a1cae" />

Check trong telegram

<img width="1279" height="709" alt="{EBFED84E-79C0-4921-8D57-DF0F81F7F44C}" src="https://github.com/user-attachments/assets/c599df99-c61e-431b-94a7-295fa26c0a55" />
 
Từ cảnh báo ở telegram, gửi lệnh chặn IP tấn công

http://192.168.182.145:3001[copy lệnh bôi đen là chặn]

Sau khi đã chặn IP tấn công, ping thử lại từ máy kali đến máy victim thì thấy không ping được nữa.
 
<img width="1278" height="706" alt="{D7B50DCE-89F3-4B4A-A52A-E03CB02833BA}" src="https://github.com/user-attachments/assets/aa8ea96d-e1ba-4b62-8b09-4a4bad0c5055" />

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

 
