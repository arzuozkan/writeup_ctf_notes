# Crackme1

Verilen dosyayı indirip çalıştırınca bize flag değerini veriyor. Ama neden? :D

Dosyanın türünü mimarisini, file komutu ile kontrol edilebilir.  

![](pics/Pasted%20image%2020230129120104.png)


64bit ELF ve not stripped. 

![](pics/Pasted%20image%2020230129131540.png)


# Crackme2

Program çalışacağı zaman bir parola olarak argüman almaktadır. 

![](pics/Pasted%20image%2020230129164936.png)

Burada disassembler aracı olarak radare2 kullandım. 

>r2 -d crackme2

komutu ile programı debugger modda (-d parametresi bunun için kullanılır) başlatabiliriz.

>aaa , komutu analizi gerçekleştirir

![](pics/Pasted%20image%2020230129165641.png)

> afl komutunu kullanarak da fonksiyonları listeleyebiliriz.

Program içerisinde printf(), strcmp() gibi hazır fonksiyonların kullanıldığını söyleyebiliriz. giveFlag() isimli fonksiyon da bulunmaktadır.

![](pics/Pasted%20image%2020230129170052.png)

Program akışına bakacak olursak,

> pdf@main 

komutu main fonksiyonunun assembly halini gösterir. (pdf=print disassembly at function)

![](pics/Pasted%20image%2020230129164648.png)

Kullanıcıdan aldığı değer ile super_secret_password değerini karşılaştırmaktadır (call sym.imp.strcmp) . strcmp metodun return ettiği değer eax. içerisinde tutulur ama eax'ın değerini değiştirmez zero flag ZF değerini değiştirir.

> `test eax,eax` 
> `je 0x8048504 # ZF=1 ise verilen adrese gider.` 

Devamında da giveFlag() metodunu çağırır ve program sonlanır.

![](pics/Pasted%20image%2020230129173258.png)

program içerisinde bulunan string değerini kullanarak çalıştırdığımız zaman flag değerine ulaşmış olduk.

![](pics/Pasted%20image%2020230129173557.png)

# Crackme3 

![](pics/Pasted%20image%2020230129201440.png)

Dosyanın stripped olması reversing ve debugging işleminde main fonksiyonun adresini bulma kısmını zorlaştıracaktır. gdb veya ghidra gibi araçlarda fonksiyonlar içerisinde main görünmeyecektir.

> `__libc_start_main` metodun ilk parametresi main() fonksiyonu çağırmaktadır.   

![](pics/Pasted%20image%2020230129225941.png)

>`PUSH FUN_080484f4` instruction ile main() fonksiyonunun olduğunu anlayabiliriz. 

Aynı zamanda decompile penceresinde de Ghidra aracın oluşturmuş olduğu kodları inceleyebiliriz.

![](pics/Pasted%20image%2020230129230827.png)

Fonksiyona çift tıklayarak bulunduğu yere gidebiliriz. İçeriği incelendiğinde main() fonksiyon olduğu anlaşılabilir. 

![](pics/Pasted%20image%2020230129231006.png)

fonksiyon içerisinde parolayı kontrol eden koşulun içerisinde karşılaştırdığı değer base64 ile decode edilmiştir.  Komut satırından base64 aracını kullanarak veya cyberchef aracı ile de decode edilebilir. Ortaya çıkan değer flag değeridir.

![](pics/Pasted%20image%2020230129231435.png)

# Crackme4

![](pics/tmp.png)

main() içerisinde yeterli sayıda argüman olup olmadığı kontrol edildikten sonra parola doğrulama kısmı compare_pwd() metodu ile başlamaktadır.

![](pics/tmp%201.png)

ghidra main decompile penceresinde de main kodlarını görebiliriz.

![](pics/Pasted%20image%2020230130160408.png)

compare_pwd() fonksiyonu içerisinde get_pwd() ile parola değerini çağırır ve kullanıcıdan aldığı değeri karışlaştırır.

![](pics/Pasted%20image%2020230130170201.png)

get_pwd() metodunda döngü içerisinde bazı işlemler gerçekleşmiş .

![](pics/Pasted%20image%2020230130164613.png)

Burada get_pwd metodu içerisine girmeden compare_pwd içerisinde parolanın döndürdüğü get_pwd() fonksiyonu ekrana yazdıracak şekilde programı patchledim. Burada parola değeri local_28 adresine yazılmaktadır. compare_pwd() metodu içerisindeki eşitliğin sağlanmadığı else durumunda 

![](pics/Screenshot%20(40).jpg)

compare_pwd() metodu içerisindeki eşitliğin sağlanmadığı else durumunda kullanıcıdan alınan değer yerine parolayı vermektedir. Kodun son hali şekildeki gibidir.

![](pics/Pasted%20image%2020230130170544.png)

Sonrasında yapılan değişiklikleri kaydetmek için File -> Export  Program ile dosyayı dışa aktarabiliriz. Programa çalıştırma izni vererek çalıştırıldığında, bize parola değerini verecektir.

![](pics/Pasted%20image%2020230130170945.png)


# Kaynak
- https://refspecs.linuxbase.org/LSB_3.1.0/LSB-generic/LSB-generic/baselib---libc-start-main-.html
- [Liveoverflow-Finding main() in Stripped Binary - bin 0x2C](https://www.youtube.com/watch?v=N1US3c6CpSw)

