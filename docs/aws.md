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