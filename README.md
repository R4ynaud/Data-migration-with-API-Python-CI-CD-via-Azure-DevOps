# Data-migration-with-API-Python-scheduled-via-Azure-DevOps
Data migration with API (Python), scheduled via Azure DevOps


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

```
import pyodbc
from fastapi import FastAPI

app = FastAPI()

# Source & Target DB Connection

source_conn = pyodbc.connect(
    'DRIVER={SQL Server};SERVER=source_server;DATABASE=Work;UID=user;PWD=password'
)
target_conn = pyodbc.connect(
    'DRIVER={SQL Server};SERVER=target_server;DATABASE=Target;UID=user;PWD=password'
)

# Data migration function
def transfer_data():
    source_cursor = source_conn.cursor()
    target_cursor = target_conn.cursor()

    source_cursor.execute('SELECT * FROM Customers')
    rows = source_cursor.fetchall()

    for row in rows:
        # CustomerID check in target database
        target_cursor.execute('SELECT COUNT(*) FROM Target_Customers WHERE CustomerID = ?', row.CustomerID)
        if target_cursor.fetchone()[0] == 0:
            # Add CustomerID if it is not in the target database
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

# Starting the app
if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="0.0.0.0", port=8000)

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

![image](https://github.com/user-attachments/assets/7eb2478b-6571-41cb-9b8d-5da2b3b11c5d)


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
git remote set-url origin https://dev.azure.com/your_organization/your_project/_git/your_repo
```

![image](https://github.com/user-attachments/assets/1580f6ce-4b95-449a-a2a4-06c7a87b9899)


## We push the repository we created locally to Azure DevOps using the following git command.

## Local olarak oluşturduğumuz repository’yi aşağıdaki git komutunu kullanarak Azure DevOps’a push ediyoruz.


• If you are doing this for the first time, it will ask you to enter your username and password !

• Eğer bu işlemi ilk kez yapıyorsanız sizden kullanıcı adınızı ve şifrenizi girmenizi isteyecektir !


```
git push origin master
```

![image](https://github.com/user-attachments/assets/67d3d4de-fef4-4b71-b9a2-d3d98f75fdb5)





