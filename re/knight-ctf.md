# RE

## Help Jimmy

![](pics/Pasted%20image%2020230121155135.png)

Bağlantıda verilen dosyayı indirip çalıştırdığım zaman 2 seçenek vermektedir. Farklı bir seçenek girildiği zaman geçersiz değer mesajı ile karşılaşılmaktadır.

![](pics/Pasted%20image%2020230121155109.png)

#TODO devam edilecek ...


## Stegrov

![](pics/Pasted%20image%2020230121155412.png)

Bağlantıda bir resim dosyası verilmiştir.

![](pics/Pasted%20image%2020230121155503.png)

Dosyayı indirip stegseek aracı ile içerisinde gizlenmiş dosya çıkartılabilir. steghide aracını kullanmıştım ve gizli dosyayı extract etmek için passphrase soruyor. Stegseek aracı ile hem passpharese  kırıp hem de içerisindekileri extract edebiliyor.

![](pics/Pasted%20image%2020230121155918.png)
kctf.jpg.out şeklinde bir dosya çıkartmış oldu. Bu dosya ELF dosyası olduğunu file komutu ile öğrenebiliriz.

![](pics/Pasted%20image%2020230121160108.png)

Sonrasında reverse işlemlerini yapabiliriz. Ghidra ile açtım ama hiçbir şey anlamadım (as a beginner :D) Main fonksiyonu aradım onu da bulamadım.
Dosyayı çalıştırmayı denedim, çalıştırma izni verdim 

> chmod +x dosya_ismi 

komutu ile. 
![](pics/Pasted%20image%2020230121160729.png)

Dosyanın içerdiği stringlere bakalım

>strings kctf.jpg.out

![](pics/Pasted%20image%2020230123155212.png)

#TODO devamı gelicek...