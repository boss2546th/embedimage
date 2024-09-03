

### 1. **โครงสร้างของ Lab และทีมต่างๆ**
ในห้องแล็บนี้มีการแบ่งงานออกเป็น 4 ทีมหลัก:
- **Infrastructure Team:**
  - หน้าที่หลักคือการจำลองระบบ server หรือ network ของศาลและธนาคารขึ้นมา โดยใช้ hardware ภายในห้อง lab
  - ระบบที่จำลองขึ้นมาใช้เพื่อทดสอบเจาะระบบ และเก็บข้อมูลที่เกี่ยวข้องเพื่อใช้วิเคราะห์ด้านความปลอดภัยทางไซเบอร์

- **Penetration Testing (Pentest) Team:**
  - ทีมนี้เน้นการทดสอบเจาะระบบด้วยเครื่องมือและวิธีการต่าง ๆ 
  - มีการทดสอบในหลายรูปแบบ เช่น การทดสอบช่องโหว่ การทดสอบความแข็งแรงของระบบ เป็นต้น

- **Wireless Hacking Team:**
  - ทีมนี้เป็นส่วนหนึ่งของ Pentest Team แต่เน้นเฉพาะการเจาะระบบผ่านเครือข่ายไร้สาย
  - มีการทดสอบการเข้าถึงข้อมูลและการโจมตีผ่านสัญญาณ Wi-Fi เป็นต้น

- **Data and AI Team:**
  - ทีมนี้รับผิดชอบการจัดเก็บและวิเคราะห์ข้อมูลที่เกี่ยวข้อง เช่น log จากระบบ, network traffic
  - ใช้ข้อมูลเหล่านี้ในการพัฒนา AI เพื่อทำนายการโจมตีทางไซเบอร์ในรูปแบบต่าง ๆ

### 2. **การจัดการข้อมูลและการเชื่อมต่อกับ Google Cloud**
เนื่องจากแล็บกำลังได้รับการสนับสนุนจาก Google Cloud เราสามารถใช้บริการของ Google Cloud เพื่อเพิ่มประสิทธิภาพในการทำงานของทีมต่างๆ:
- **Infrastructure Team:**
  - **Google Compute Engine**: ใช้สำหรับสร้างระบบ server จำลองบนคลาวด์แทนการใช้ hardware ภายในแล็บ
    - คุณสมบัติ: สร้าง virtual machines (VMs) ที่สามารถปรับขนาดได้ตามความต้องการของงาน
    - ตัวอย่างคำสั่ง: 
      ```shell
      gcloud compute instances create my-instance \
        --zone=us-central1-a \
        --machine-type=e2-medium \
        --image-family=debian-10 \
        --image-project=debian-cloud
      ```

  - **Google Kubernetes Engine (GKE)**: ใช้จัดการ containerized applications ที่จำลองระบบ server ขนาดใหญ่
    - คุณสมบัติ: ระบบที่ช่วยในการจัดการการรัน container บนคลัสเตอร์ Kubernetes
    - ตัวอย่างคำสั่ง:
      ```shell
      gcloud container clusters create my-cluster \
        --num-nodes=3 \
        --zone=us-central1-a
      ```

- **Pentest และ Wireless Hacking Teams:**
  - **Google Cloud VPN**: ใช้ในการตั้งค่าเชื่อมต่อ VPN เพื่อจำลองการเชื่อมต่อจากภายนอก
    - คุณสมบัติ: เชื่อมต่อเครือข่ายภายในของคุณกับ Google Cloud ผ่านการเข้ารหัส
    - ตัวอย่างคำสั่ง: 
      ```shell
      gcloud compute vpn-tunnels create my-vpn-tunnel \
        --region us-central1 \
        --peer-ip 35.191.0.2 \
        --ike-version 2 \
        --shared-secret 'my-secret'
      ```

  - **Cloud Armor**: ใช้ป้องกันการโจมตี DDoS และการโจมตีอื่น ๆ จากเครือข่ายภายนอก
    - คุณสมบัติ: ใช้สำหรับการปกป้องแอปพลิเคชันจากการโจมตีบนเว็บ
    - ตัวอย่างการตั้งค่า:
      ```shell
      gcloud compute security-policies create my-security-policy \
        --description "Security policy for my project"
      ```

- **Data and AI Team:**
  - **Google Cloud Storage**: ใช้จัดเก็บข้อมูลขนาดใหญ่ เช่น log, network traffic
    - คุณสมบัติ: การจัดเก็บข้อมูลในรูปแบบ object storage ที่สามารถเข้าถึงได้ทั่วโลก
    - ตัวอย่างคำสั่ง: 
      ```shell
      gsutil mb gs://my-data-bucket
      ```

  - **BigQuery**: ใช้สำหรับวิเคราะห์และดึงข้อมูลขนาดใหญ่
    - คุณสมบัติ: ระบบจัดเก็บข้อมูลและประมวลผลที่สามารถวิเคราะห์ข้อมูลขนาดใหญ่ในเวลาที่รวดเร็ว
    - ตัวอย่างคำสั่ง:
      ```shell
      bq query --use_legacy_sql=false \
      'SELECT name, SUM(number) as total FROM `my_project.my_dataset.my_table` GROUP BY name'
      ```

  - **AI Platform**: ใช้สำหรับฝึกและ deploy โมเดล machine learning
    - คุณสมบัติ: แพลตฟอร์มที่ช่วยในการฝึกโมเดล AI และนำไปใช้งานได้อย่างง่ายดาย
    - ตัวอย่างคำสั่ง:
      ```shell
      gcloud ai-platform jobs submit training my_job \
        --module-name trainer.task \
        --package-path trainer/ \
        --region us-central1 \
        --runtime-version 2.3 \
        --python-version 3.7
      ```

### 3. **Port Mirroring และการจัดเก็บข้อมูล**
การตั้งค่า Port Mirroring ภายในแล็บทำให้สามารถส่งข้อมูลจาก PC1 (ของทีม Infrastructure) ไปยัง PC2 (ของทีม AI):
- **โครงสร้างการเชื่อมต่อ:**
  - Switch1 (Cisco 2950) เชื่อมต่อกับ PC1, Raspberry Pi และ PC2
  - Port Mirroring ถูกตั้งค่าให้ส่งข้อมูลจาก PC1 ไปยัง PC2

- **ผลลัพธ์ของ Port Mirroring:**
  - บน PC2 จะสามารถเห็นข้อมูลที่ถูกส่งผ่านจาก PC1 เช่น network traffic
  - ข้อมูลที่สามารถดูได้รวมถึง Snort data และ access logs จาก Apache server บน PC1

- **การตั้งค่า Port Mirroring บน Cisco 2950**:
  - ขั้นตอน: Port Mirroring หรือการทำ SPAN (Switch Port Analyzer) บน Cisco Switch 2950 ช่วยให้คุณสามารถกำหนดพอร์ตหนึ่งเพื่อสะท้อนทราฟฟิกจากพอร์ตอื่น ๆ ได้
  - ตัวอย่างคำสั่ง:
    ```shell
    configure terminal
    monitor session 1 source interface fastethernet 0/1
    monitor session 1 destination interface fastethernet 0/2
    end
    ```

### 4. **การวิเคราะห์และการพัฒนา AI**
- **การใช้ Wireshark หรือ Tcpdump**:
  - **Wireshark**: เป็นเครื่องมือที่ช่วยในการจับและวิเคราะห์ network traffic แบบกราฟิก
    - คุณสมบัติ: ใช้ดูแพ็กเก็ตแบบเรียลไทม์, ฟิลเตอร์ข้อมูล, และวิเคราะห์โปรโตคอลต่าง ๆ
    - วิธีการใช้งาน:
      1. เปิด Wireshark และเลือกอินเทอร์เฟซที่ต้องการจับข้อมูล
      2. ใช้ฟิลเตอร์เช่น `http` หรือ `tcp.port == 80` เพื่อกรองข้อมูลเฉพาะที่ต้องการดู
      3. สามารถบันทึกข้อมูลเป็นไฟล์ .pcap สำหรับการวิเคราะห์ภายหลังได้

  - **Tcpdump**: เป็นเครื่องมือ command-line สำหรับจับ network traffic
    - คุณสมบัติ: ใช้จับข้อมูลเครือข่ายในรูปแบบ command-line สามารถใช้ฟิลเตอร์เพื่อกรองข้อมูลที่สนใจ
    - ตัวอย่างคำสั่ง:
      ```shell
      sudo tcpdump -i eth0 -w output.pcap
      sudo tcpdump -i eth0 'tcp port 80' -w http_traffic.pcap
      ```

- **การนำข้อมูลขึ้น Google Cloud**:
  - สามารถใช้ SCP หรือ Rsync ในการย้ายข้อมูลจาก PC2 ไปยัง Google Cloud Storage
    - **SCP** (Secure Copy Protocol):
      - คำสั่ง SCP ใช้ในการโอนย้ายไฟล์ระหว่างเครื่องที่ปลอดภัยผ่าน SSH
      - ตัวอย่างคำสั่ง:
        ```shell
        scp localfile.txt username@remotehost:/path/to/remote/directory/
        ```

    - **Rsync**:
      - Rsync ใช้ในการซิงค์ไฟล์และโฟลเดอร์ระหว่างเครื่อง
      - คำสั่ง Rsync ยังช่วยในการถ่ายโอนไฟล์ที่มีขนาดใหญ่หรือการ backup ข้อมูล
      - ตัวอย่างคำสั่ง:
        ```shell
        rsync -avz /path/to/source/ username@remotehost:/path/to/destination/
        ```

  - จากนั้นสามารถใช้ BigQuery ในการวิเคราะห์ข้อมูล และใช้ AI Platform ในการพัฒนาโมเดล AI

### 5. **เครื่องมือและเทคนิคต่าง ๆ** (ต่อ)
- **SCP** (Secure Copy Protocol):
  - ใช้ในการย้ายไฟล์ระหว่างเครื่องที่ปลอดภัยผ่าน SSH
  - เหมาะสำหรับการย้ายไฟล์เดี่ยวหรือโฟลเดอร์
  - ตัวอย่างคำสั่ง:
    ```shell
    scp /path/to/local/file.txt username@remotehost:/path/to/remote/directory/
    ```
  - คำสั่งนี้จะย้าย `file.txt` จากเครื่อง local ไปยังเครื่อง remote โดยใช้ SSH

- **Rsync**:
  - Rsync ใช้ในการซิงค์ไฟล์และโฟลเดอร์ระหว่างเครื่อง โดยสามารถเลือกเฉพาะไฟล์ที่เปลี่ยนแปลงเท่านั้นเพื่อเพิ่มประสิทธิภาพในการถ่ายโอนข้อมูล
  - เหมาะสำหรับการ backup ข้อมูลหรือการซิงค์ข้อมูลขนาดใหญ่
  - ตัวอย่างคำสั่ง:
    ```shell
    rsync -avz /path/to/source/ username@remotehost:/path/to/destination/
    ```
  - คำสั่งนี้จะซิงค์ข้อมูลจากโฟลเดอร์ `source` บนเครื่อง local ไปยังโฟลเดอร์ `destination` บนเครื่อง remote

- **การทำ Port Mirroring บน Cisco 2950**:
  - Port Mirroring หรือ SPAN (Switch Port Analyzer) ช่วยให้คุณสามารถกำหนดพอร์ตหนึ่งเพื่อสะท้อนทราฟฟิกจากพอร์ตอื่น ๆ ได้
  - การตั้งค่า:
    1. **เข้าสู่โหมด configuration** บน Cisco Switch
    2. **กำหนดพอร์ตต้นทาง** (ที่ต้องการสะท้อนข้อมูล) เช่น:
       ```shell
       monitor session 1 source interface fastethernet 0/1
       ```
    3. **กำหนดพอร์ตปลายทาง** (ที่ข้อมูลจะถูกส่งไป) เช่น:
       ```shell
       monitor session 1 destination interface fastethernet 0/2
       ```
    4. **บันทึกการตั้งค่า**:
       ```shell
       write memory
       ```
  
- **Wireshark**:
  - เป็นเครื่องมือกราฟิกที่ช่วยในการจับและวิเคราะห์ network traffic ในลักษณะ real-time
  - สามารถใช้กรองข้อมูลที่สนใจได้ เช่น การฟิลเตอร์เฉพาะโปรโตคอล หรือพอร์ตที่ต้องการ
  - เหมาะสำหรับการ monitor ข้อมูล network traffic แบบละเอียด

- **Tcpdump**:
  - เป็นเครื่องมือ command-line สำหรับจับ network traffic
  - ใช้คำสั่ง `tcpdump` เพื่อจับข้อมูลที่ผ่านในเครือข่าย เช่น:
    ```shell
    sudo tcpdump -i eth0 'tcp port 80' -w http_traffic.pcap
    ```
  - สามารถบันทึกข้อมูลเป็นไฟล์ `.pcap` เพื่อวิเคราะห์ภายหลังได้

### 6. **การใช้ Google Cloud ในการจัดเก็บและวิเคราะห์ข้อมูล**
- **การจัดเก็บข้อมูลแบบเรียลไทม์เพื่อการ monitor:**
  - ใช้ **Stackdriver Logging** หรือ **Pub/Sub** บน GCP เพื่อรวบรวม log แบบเรียลไทม์
  - ส่งข้อมูลไปยัง BigQuery เพื่อทำการ query ข้อมูลแบบเรียลไทม์

- **การจัดเก็บข้อมูลเพื่อการวิเคราะห์และพัฒนา AI:**
  - ใช้ **Cloud Storage** ในการเก็บข้อมูลทุกๆ 2-3 ชั่วโมง เช่น การใช้ `rsync` ในการอัปโหลดข้อมูลจาก PC2 ขึ้น Cloud Storage
  - จากนั้นใช้ **BigQuery** ในการวิเคราะห์ข้อมูลขนาดใหญ่
  - ข้อมูลที่ได้สามารถนำไปใช้ในการพัฒนาและฝึกโมเดล AI บน **AI Platform**

ทั้งหมดนี้ควรศึกษาเพิ่มเติมจากเอกสารของ Google Cloud และทดลองใช้งานจริงเพื่อทำความเข้าใจในรายละเอียดของแต่ละบริการครับ

---

ถ้ามีข้อสงสัยหรืออยากได้คำแนะนำเพิ่มเติมเรื่องไหน ผมยินดีช่วยเสมอครับ!
