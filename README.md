# Data-migration-with-API-Python-scheduled-via-Azure-DevOps
Data migration with API (Python), scheduled via Azure DevOps


![image](https://github.com/user-attachments/assets/666c3dd5-943c-4c90-a4a7-b1e35f7c835e)


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

