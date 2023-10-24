# On-Premises, IAAS, PAAS, SAAS


![image info](images/cloud/iaas-paas-saas-comparison.jpg)

## On-Premises

Her şey sana ait. Sanallaşma, depolama, işletim istemi network ayarları, hepsini sen configure etmen gerekiyor.

## IAAS (Infrastructure as a Service) 
İşletim sistemi ve alt yapını kendin kurman gerekiyor. 
*Exp:* EC2 servisi 

## PAAS (Platform as a Service)

İşletim sistemi ve alt yapı hazır. Sadece uygulamayı kurman ve yönetmen yeterli.

*Exp:* elastic beanstalk servisi

## SAAS (Software as a Service)

Tüm alt yapı ve proje hizmet aldığın firmaya aittir.
*Exp:* work docs, lambda servisi 



# Region, Availability Zone, CEL(Center Edge Location)

Region aws data center'larının bulunduğu bölgelerdir. 
Her region en az iki AZ'den (availability zone) oluşur.

**Center Edge Location:** Uzak bir bölgeden çekilen static dosyalar yakında bulunan bölgeye taşınır. Bu şekilde bi sonraki isteklerde yüksek hızlarda erişilebilir. Bu servisi sağlayan bölgelere **Edge Location** denir.

<br> 

# SSM (System Manager)


*getParameters (withDecryption)*
```bash
aws ssm get-parameters --names /dev/auth0/clients/activation-lambda/clientId --with-decryption
```
<br> 

*List All Parameters 
```bash
aws ssm describe-parameters
```

# AWS Solution Architect Associate
## Included Services
- Compute
- Storage
- Database
- Migration
- Networking and Content Delivery
- Management Tools
- Security and Identity
- Compliance ve Application Integration
---------------------


# ---------------Services--------------

# Compute
## EC2 (Elastic Compute Cloud)
AWS'in sanal makine hizmetidir.
EC2 bulutta güvenli, yeniden boyutlandırılabilen bilgi işlem kapasitesi sağlayan bir web hizmetidir. Geliştiriciler için web ölçeğindeki bulut bilişimi daha kolay hale getirmek için tasarlanmıştır.

## Ligthsail
AWS üzerinde bir özel sanal sunucuya sahip olmanın ve yönetmenin en kolay yolu olarak tasarlanmıştır. Lightsail planları, düşük öngörülebilir bir fiyatla projenize hızlı bir başlangıç yapmak için ihtiyacınız olan her şeyi (sanal makine, SSD tabanlı depolama, veri aktarımı, DNS yönetimi ve statik IP) içerir.

## ECS (Elastic Container Service)
Docker container'lerini destekleyen ve AWS'de container uygulamalarını kolayca çalıştırmanıza ve ölçeklendirmenize izin veren yüksek oranda ölçeklenebilir, yüksek performanslı bir container düzenleme hizmetidir.

Amazon ECS, kendi container düzenleme yazılımınızı kurmanız ve çalıştırmanız, bir sanal makine kümnesini, yönetmeniz ve ölçeklendirmeniz veya bu sanal makinelerdeki container'ları programlamanız gereğini ortadan kaldırır.

## EKS (Elastic Kubernetes Service)
AWS'de Kubernates kullanarak container'li uygulamaların dağıtımını, yönetilmesini ve ölçeklendirilmesini kolaylaştırır.

## Lambda
Serverless'tir.  
Sunucu yönetme zahmetine girmeden kod çalıştırmanıza izin verir. Yanlızca tükettiğiniz hesaplama zaman için ödeme yaparsınız. Kodunuz çalışmadığında ücret alınmaz.

Lambda ile hemen hemen her türlü uygulama veya arka uç hizmeti için kod çalıştırabilirsiniz. Sadece kodunuzu yükleyin ve Lambda, yüksek kullanılabilirlikle kodunuzu çalıştırmak ve ölçeklendirmek için gereken her şeyi halleder. Kodunuzu diğer AWS hizmetlerinden otomatik olarak tetiklemek veya doğrudan herhangi bir web veya mobil uygulamadan çağırmak için ayarlayabilirsiniz.

## Batch
Geliştiricilerin, bilim adamlarının ve mühendislerin AWS'de yüzbinlerce bilgisayar işlemlerini kolayca ve verimli bir şekilde çalıştırmasını sağlar.

## Elastic Beanstalk

PAAS gibi, kodunuzu yazıp servise yüklemeniz yeterli, sunucu ayarları ile ilgilenmenize gerek kalmıyor. Serverless'tan bir adım önceki modeldir.

# Storage
## S3  (Simple Storage Service)
- Obje tabanlı depolama sistemidir.
- Herhangi bir ortamdan gelen, herhangi bir miktardaki veriyi depolamak ve almak için oluşturulmuş bir nesne deposudur. %99.999 dayanıklılık ve sağlamlık sağlamak için tasarlanmıştır.
- Yüklenebiliecek tek bir dosyanın sınırı 5TB dır.
- S3 e koyulabilecek dosya adeti ve toplam büyüklükte ise bir sınır yoktur.
