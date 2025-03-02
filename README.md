# Data-migration-with-API-Python-CI/CD-via-Azure-DevOps
Data migration with API (Python), CI/CD via Azure DevOps


![image](https://github.com/user-attachments/assets/02b5c1d3-7e6f-4961-9018-b1a5cccd7712)



### What is an API ?

An API (Application Programming Interface) is an interface that enables different software components to communicate and exchange data with each other. APIs expose specific functionalities or datasets to the outside world, allowing applications to integrate and work together seamlessly.

### Developing APIs with Python

In this documentation, we will develop APIs using Python. Python's extensive library support, flexibility, and ease of use accelerate and simplify the API development process. Frameworks like Flask or Django are particularly effective for creating APIs quickly and efficiently.

### Advantages of Data Transfer with API

• Seamless Integration: APIs allow different systems and applications to connect smoothly.

• Automation: Using APIs eliminates manual data entry and enables process automation.

• Flexibility: Supports various data formats and protocols, making it applicable in a wide range of scenarios.

• Efficiency: Facilitates fast and reliable data transfer, enhancing operational efficiency.

### Automating the Entire Process on Azure DevOps
 
Azure DevOps is a powerful platform for automating software development, testing, and deployment processes. By integrating Python-based APIs, processes such as continuous integration (CI), continuous delivery (CD), test automation, and monitoring can be fully automated. This reduces the risk of errors, speeds up the process, and improves software quality.



### API Nedir ?

API (Application Programming Interface), farklı yazılım bileşenlerinin birbirleriyle iletişim kurmasını ve veri alışverişinde bulunmasını sağlayan bir arayüzdür. API'lar, belirli işlevleri veya veri setlerini dış dünyaya açarak, uygulamaların birbirleriyle entegre olmasını ve birlikte çalışmasını mümkün kılar.

### Python ile API Geliştirme

Bu dokümanda, Python kullanarak API geliştireceğiz. Python'un geniş kütüphane desteği, esnekliği ve kullanım kolaylığı, API geliştirme süreçlerini hızlandırır ve kolaylaştırır. Özellikle Flask veya Django gibi web framework'leri, hızlı ve etkili bir şekilde API oluşturmayı sağlar.

### API ile Veri Aktarmanın Avantajları

• Kolay Entegrasyon: API'lar, farklı sistemlerin ve uygulamaların sorunsuz bir şekilde birbirine bağlanmasını sağlar.

• Otomasyon: API kullanımı, manuel veri girişini ortadan kaldırarak süreçlerin otomatikleşmesini sağlar.

• Esneklik: Farklı veri formatlarını ve protokollerini destekleyerek geniş bir uygulama yelpazesinde kullanılabilir.

• Verimlilik: Hızlı ve güvenilir veri transferi imkanı sunarak operasyonel verimliliği artırır.

### Azure DevOps Üzerinde Tüm Sürecin Otomatize Edilmesi

Azure DevOps, yazılım geliştirme, test etme ve dağıtım süreçlerini otomatize eden güçlü bir platformdur. Python tabanlı API'ların entegrasyonu ile sürekli entegrasyon (CI), sürekli teslimat (CD), test otomasyonu ve izleme gibi süreçler tamamen otomatize edilebilir. Bu, hata riskini azaltır, sürecin hızını artırır ve yazılım kalitesini iyileştirir.


## First, we install Python on our computer to develop our API.

## Öncelikle API’mizi geliştirmek için bilgisayarımıza Python kuruyoruz.

• Download and install the latest version of Python from the following link.

• Aşağıdaki bağlantıdan Python'un en son sürümünü indirip yükleyin.


https://www.python.org/downloads/windows/


## To check if Pip is installed correctly, run the following command.

## Pip'in doğru kurulup kurulmadığını kontrol etmek için aşağıdaki komutu çalıştırın.

```
pip --version
```

![image](https://github.com/user-attachments/assets/22e8ce6b-41b6-490b-9855-f0cdc02bec10)


## We need to install FastAPI and Uvicorn to create the API.

## API’yi oluşturmak için FastAPI ve Uvicorn kurmamız gerekiyor.

```
pip install fastapi uvicorn
```

![image](https://github.com/user-attachments/assets/9c8b50f7-1c2e-4beb-b687-d963d93d9328)


## After completing the installations, use an editor to convert the following Python code into a .py file.

## Kurulumları tamamladıktan sonra aşağıdaki Python kodunu bir editör kullanarak .py dosyasına dönüştürün.


# Note; 

• The API we have developed will provide data integration between the source database and the target database, and will ensure data consistency checks every 20 seconds. It will allow us to pull data into the target database as soon as it is entered into the source database.

# Not; 

• Geliştirdiğimiz API, kaynak veritabanı ile hedef veritabanı arasında veri entegrasyonunu sağlayacak ve her 20 saniyede bir veri tutarlılık kontrollerinin yapılmasını sağlayacaktır. Kaynak veritabanına girildiği anda verileri hedef veritabanına çekmemize olanak tanıyacaktır. 



```
import pyodbc
from fastapi import FastAPI
import uvicorn
import requests
import threading

app = FastAPI()

# db_connection
source_conn = pyodbc.connect(
    'DRIVER={SQL Server};SERVER=SERVER-NAME\\NAME;DATABASE=DB_NAME;UID=USER;PWD=PASSWORD'
)
target_conn = pyodbc.connect(
    'DRIVER={SQL Server};SERVER=SERVER-NAME\\NAME;DATABASE=DB_NAME;UID=USER;PWD=PASSWORD'
)

# Data Migration function
def transfer_data():
    source_cursor = source_conn.cursor()
    target_cursor = target_conn.cursor()

    source_cursor.execute('SELECT * FROM Customers')
    rows = source_cursor.fetchall()

    for row in rows:
        # Target DB CustomerID control
        target_cursor.execute('SELECT COUNT(*) FROM Target_Customers WHERE CustomerID = ?', row.CustomerID)
        if target_cursor.fetchone()[0] == 0:
            # CustomerID add it if it is not in the target database
            target_cursor.execute('''
                INSERT INTO Target_Customers (CustomerID, CompanyName, ContactName, ContactTitle, Address, City, Region, PostalCode, Country, Phone, Fax)
                VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?, ?)
            ''', row.CustomerID, row.CompanyName, row.ContactName, row.ContactTitle, row.Address, row.City, row.Region, row.PostalCode, row.Country, row.Phone, row.Fax)

    target_conn.commit()
    source_cursor.close()
    target_cursor.close()

# FastAPI endpoint'i
@app.get("/transfer")
def transfer_endpoint():
    transfer_data()
    return {"status": "success", "message": "Data transferred successfully"}

# API to automatically transfer data when started
def auto_transfer():
    while True:
        response = requests.get("http://localhost:8000/transfer")
        print(response.json())
        time.sleep(20)  # 20 wait a second

# App
if __name__ == "__main__":
    # Uvicorn
    server_thread = threading.Thread(target=uvicorn.run, args=(app,), kwargs={"host": "0.0.0.0", "port": 8000})
    server_thread.start()

    # API add a small delay to trigger data transfer after initialization
    import time
    time.sleep(2)  # 2 wait a second
    threading.Thread(target=auto_transfer).start()

```

## To convert the API we created into an executable (.exe), run the following commands in order.

## Oluşturduğumuz API’yi çalıştırılabilir (.exe) bir dosyaya dönüştürmek için aşağıdaki komutları sırasıyla çalıştırın.

```
pip install pyinstaller
```

![image](https://github.com/user-attachments/assets/2b43901f-81de-4dc1-ae56-c108e7356f38)


```
pyinstaller --onefile Data.py
```

![image](https://github.com/user-attachments/assets/9b3e28ef-cdf0-45e9-a6ba-6962b12ca0da)


## The API we converted to an executable (.exe) will appear as follows.

## .exe'e dönüştürdüğümüz API aşağıdaki gibi görünecektir


![image](https://github.com/user-attachments/assets/ed58b1c1-2887-473d-9fa2-d62518cb396b)


## Finally, after running the executable (.exe), we check if the data has been transferred correctly, first from our browser and then from the database.

## Data.exe'i çalıştırdıktan sonra, önce tarayıcımızdan, ardından veritabanından verilerin doğru şekilde aktarılıp aktarılmadığını kontrol ediyoruz.


![image](https://github.com/user-attachments/assets/161f638e-4d97-4bdd-9ec2-68aebbcac823)


## The information in the table below belongs to the Microsoft Northwind database !

## Aşağıdaki tabloda yer alan bilgiler Microsoft Northwind veritabanına aittir !

```
https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/northwind-pubs  
```

![image](https://github.com/user-attachments/assets/1c6bc4a5-86ec-4bbe-9979-363207a8e27b)


# Azure DevOps configuration.

We have developed and tested our API, ensuring seamless data transfer. However, to run this API continuously, we need to initiate a CI/CD process in Azure DevOps to automate all these processes. This ensures uninterrupted and error-free data transfer operations. CI/CD integration allows for instant application of code changes and rapid deployment of new versions, thereby increasing operational efficiency.

# Azure DevOps yapılandırması. 

API'mizi oluşturduk ve test ettik; sorunsuz veri aktarımı gerçekleştiriyor. Ancak, bu API'yi sürekli olarak çalıştırmak için Azure DevOps'ta bir CI/CD süreci başlatarak tüm bu süreçleri otomatikleştirmeliyiz. Bu sayede, veri aktarımı işlemleri kesintisiz ve hatasız bir şekilde gerçekleştirilir. CI/CD entegrasyonu, kod değişikliklerinin anında uygulanmasını ve yeni sürümlerin hızlı bir şekilde dağıtılmasını sağlar, böylece operasyonel verimlilik artar.


## First, we convert our local project into a Git repository.

## Öncelikle yerel projemizi Git deposuna dönüştürüyoruz.

```
cd C:\Users\Administrator\Desktop\Azure
```

```
git init
```

![image](https://github.com/user-attachments/assets/1301fde0-af24-4943-863f-b433ac24a85b)



## We create a new repository on Azure DevOps to transfer our project.

## Projemizi aktarmak için Azure DevOps üzerinde yeni bir repository oluşturuyoruz.


![image](https://github.com/user-attachments/assets/b41ed4d4-f268-4a1b-beaf-3f53a17a1255)


![image](https://github.com/user-attachments/assets/ead4b5fb-561d-44cf-ab44-0a2dde46cf91)


## After creating a new repository, Azure DevOps will provide you with the repository URL. You can use this URL to connect the repository to Azure DevOps.

## Yeni bir repository oluşturduktan sonra, Azure DevOps size repository URL'sini verecektir. Bu URL'yi kullanarak repository'i Azure DevOps'a bağlayabilirsiniz.



## Go to the directory of the repository we created.

## Oluşturduğumuz repository dizinine gidin.


```
cd C:\Users\Administrator\Desktop\Azure
```

## Set up the remote configuration using the Azure DevOps repository URL.

## Azure DevOps repository URL'sini kullanarak remote ayarını yapın.

```
git remote add origin https://dev.azure.com/your_organization/your_project/_git/your_repo
```

![image](https://github.com/user-attachments/assets/cac7f52d-0856-4908-9df1-1c0c098c1eac)



## We push the repository we created locally to Azure DevOps using the following git command.

## Local olarak oluşturduğumuz repository’yi aşağıdaki git komutunu kullanarak Azure DevOps’a push ediyoruz.


• If you are doing this for the first time, it will ask you to enter your username and password !

• Eğer bu işlemi ilk kez yapıyorsanız sizden kullanıcı adınızı ve şifrenizi girmenizi isteyecektir !


```
git push origin master
```

![image](https://github.com/user-attachments/assets/67d3d4de-fef4-4b71-b9a2-d3d98f75fdb5)


![image](https://github.com/user-attachments/assets/98fde850-807a-482d-8a52-55b718aad580)


## Creating a CI (Continuous Integration) Pipeline.

## CI (Continuous Integration) Pipeline'ı Oluşturma.


• In the Azure DevOps portal, go to the 'Pipelines' section in the left menu and click on the 'Create Pipeline' button.

• Azure DevOps portalında, sol menüde 'Pipelines' sekmesine gidin ve 'Create Pipeline' butonuna tıklayın.


![image](https://github.com/user-attachments/assets/fac947b7-6d6d-4867-8a72-e441a12684f6)

![image](https://github.com/user-attachments/assets/07130bad-af2e-4eaf-af9b-41511bc393be)


• For the 'Where is your code ?' question, select the 'Azure Repos Git' option.

• 'Where is your code ?' sorusuna "Azure Repos Git" seçeneğini seçin.

![image](https://github.com/user-attachments/assets/b060f8f6-fdd8-474f-96c2-1d22fa1542db)


• Select your repository.

• Repository'nizi seçin.

![image](https://github.com/user-attachments/assets/502bcbd3-5ae0-4b65-b489-7b03bafc8868)


• In the 'Configure your pipeline' step, select the 'Starter pipeline' option and edit the following YAML file.

• 'Configure your pipeline' adımında, 'Starter pipeline' seçeneğini seçin ve aşağıdaki YAML dosyasını düzenleyin

![image](https://github.com/user-attachments/assets/c23b069e-ad59-4cb9-89ec-54f578a5298a)


## For every operation you perform locally, you need to create an Azure DevOps Self-Hosted Agent!

## Local'de yapacağınız her işlem için Azure DevOps Self Hosted Agent oluşturmak zorundasınız ! 


• To run our project on Azure DevOps, we need to create a YAML file. Use the following code to create the YAML file.

• Projemizi Azure DevOps üzerinde çalıştırmak için YAML dosyası oluşturmamız gerekiyor. YAML dosyasını oluşturmak için aşağıdaki kodu kullanın.


```
trigger:
- main

pool:
  name: 'R4'  # Agent pool name

jobs:
- job: LocalAgentJob
  displayName: 'Run Python API on Local Agent'

  steps:
  - script: |
      python --version
    displayName: 'Check Python Version'

  - script: |
      python -m venv venv
      source venv/bin/activate
      pip install fastapi uvicorn requests pyodbc
    displayName: 'Set up Python environment'

  - script: |
      cd Azure
      python Data.py &
    displayName: 'Start the Python API'


```

## When the pipeline we created runs smoothly, it will produce an output similar to the following.

## Oluşturduğumuz Pipeline sorunsuz çalıştığında aşağıdakine benzer bir çıktı üretecektir.

![image](https://github.com/user-attachments/assets/0c2977e8-4cca-461c-813a-7f7e15d5a7b1)


![image](https://github.com/user-attachments/assets/33d82674-aadf-4ffa-a697-75d954cae165)


## After completing the entire process, there is only one step remaining: scheduling the pipeline we created via the Agent.

## Tüm süreci tamamladıktan sonra geriye tek bir adım kalıyor: Agent aracılığıyla oluşturduğumuz pipeline'ı schedule etmek.


## Azure DevOps agents are essential for automating and streamlining CI/CD processes. They run tasks in build, test, and deployment pipelines, ensuring consistency and efficiency.


## Importance of Azure DevOps Agents:


• Automation: Reduces manual intervention and errors.

• Scalability: Handles larger workloads with multiple agents.

• Consistency: Ensures uniform execution across environments.

• Resource Management: Optimizes hardware and software use.

• Flexibility: Supports various operating systems and configurations.


## Advantages of Azure DevOps Agents:


• Ease of Integration: Smooth integration with Azure DevOps services.

• Cost Efficiency: Manages infrastructure costs with self-hosted agents.

• Customization: Allows specific tools and scripts for unique needs.

• Improved Performance: Faster build and deployment times.

• Security: Provides robust security features for build environments.

• In summary, Azure DevOps agents are crucial for efficient, consistent, and secure DevOps workflows.


## Azure DevOps Agent'ları, CI/CD süreçlerini otomatikleştirmek ve düzenlemek için önemlidir. Derleme, test ve pipelineları çalıştırarak tutarlılık ve verimlilik sağlarlar.


## Azure DevOps Agent'larının Önemi:


• Otomasyon: Manuel müdahaleyi ve hataları azaltır.

• Ölçeklenebilirlik: Birden fazla Agent ile daha büyük iş yüklerini yönetir.

• Tutarlılık: Farklı ortamlarda tutarlı yürütme sağlar.

• Kaynak Yönetimi: Donanım ve yazılım kullanımını optimize eder.

• Esneklik: Çeşitli işletim sistemleri ve yapılandırmaları destekler.


## Azure DevOps Agent'larının Avantajları:


• Entegrasyon Kolaylığı: Azure DevOps hizmetleriyle sorunsuz entegrasyon.

• Maliyet Verimliliği: Kendi barındırdığı Agent'larla altyapı maliyetlerini yönetir.

• Özelleştirme: Özel araçlar ve komut dosyaları eklenebilir.

• Geliştirilmiş Performans: Daha hızlı derleme ve dağıtım süreleri.

• Güvenlik: Derleme ortamları için güçlü güvenlik özellikleri sağlar.

• Özetle, Azure DevOps Agent'ları, verimli, tutarlı ve güvenli DevOps iş akışları için kritik öneme sahiptir.


## First, click on the 'Pipelines' tab from the menu.

## Öncelikle menüden 'Pipelines' sekmesine tıklayın. 


![image](https://github.com/user-attachments/assets/e003b78d-5dd3-4653-88bf-228cbac1b63b)


## On the page that opens, locate the pipeline we created earlier. Click on the 'Edit' tab on the right side to enter the previously created pipeline.

## Açılan sayfada daha önce oluşturduğumuz işlem pipeline'ı bulun. Daha önce oluşturulan pipeline'a girmek için sağ taraftaki 'Edit' sekmesine tıklayın.


![image](https://github.com/user-attachments/assets/05c00144-b007-46f7-bdfd-93035a2fa74f)



## On the page that opens, click on the 'Triggers' tab located on the right side.

## Açılan sayfada sağ tarafta bulunan 'Triggers' sekmesine tıklayın.


![image](https://github.com/user-attachments/assets/df3a7d31-ebc5-48d8-8f92-38dac7c2ffde)



## Click on the '+ Add' button under the 'Scheduled' tab.

## 'Scheduled' sekmesinden '+ Add' sekmesine tıklayın.


![image](https://github.com/user-attachments/assets/2ca6e78a-66b1-4887-aa63-0facab97b08f)



## Finally, on the page that opens, we need to select the days and time range for the pipeline to run. As an example, I have scheduled it to run every day at 10:50 AM (UTC+03:00).

## Son olarak açılan sayfada pipeline'ın çalışacağı günleri ve zaman aralığını seçmemiz gerekiyor. Örnek olarak, her gün saat 10:50'de (UTC+03:00) çalışacak şekilde planladım.


![image](https://github.com/user-attachments/assets/a2408f3b-129e-4d5a-b520-d739d595d675)



## When it runs successfully, we should see an output similar to the one below, and there should be 'Scheduled' information.

## Sorunsuz çalıştığı zaman aşağıdaki gibi bir çıktı almamız gerekiyor ve 'Scheduled' bilgisi bulunması lazım.


![image](https://github.com/user-attachments/assets/1d19a4b6-267b-458f-8d20-85220cc4f150)









