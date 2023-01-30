#OpenSecurityTraining2 #re 

Kaynak:
- OpenSecuriy2 eğitim videoları ve materyalleri
- 0xinfection.github.io/reversing
- Nightmare reverse and binary exploitation - https://guyinatuxedo.github.io/index.html



## x86 Mimarisi

CPU içerisinde 4 kısım bulunur:
- Control Unit: cpu dan yönergeleri alır ve decode eder.
- Execution Unit: 
- Registers
- Flags

#eip
> EIP (Instruciton Pointer Register): önemli instructionlardan bir tanesidir. Yürütülecek bir sonraki komutu işaret eder. Eğer işaretçi değiştirilebilirse programın işleyişi de değişmektedir.

> Control registers: CPU'nun operating modunu belirleyen registerlar.

#eflags
> Flags: çalışan fonksyionun başarılı bir şekilde çalışıp çalışmadığını kontrol eder ve doğrular. Status flag, control flag ve system flag olmak üzere 3 türü bulunur.
> ![](pics/Pasted%20image%2020230127103455.png)
> 




- NOP - no operation, no registers. Hiçbir değer ifade etmez. Zaman geciktirmek için kullanılır. single byte 0x90 

 #x64 

## x64  Registers and Instructions

rbp- base pointer points bottom of the stack (in x86 >ebp)
rsp - stack pointer top op the stack
rip - instruction pointer points to the instruction to be executed

rdi: ilk argüman
rsi: ikinci argüman
rdx: üçüncü args
rcx: dördüncü 
r8: 5. argüman
r9: 6. argüman

Not : x86 mimarisinde argümanlar stack içerisinde tutulur.

### Common Instructions

>- mov source, dest 
>`mov rax, rdx # rdx'te bulunan veriyi rax'e geçirir
>`mov rax, [rdx] # rdx'in gösterdiği değeri rax'a taşır`

> - lea , ikinci operandın adresini hesaplar ve ilk operanda taşır.
>`lea rdi,[rbx+0x10] # rbx+0x10 adresini rdi'ye taşır`

> - add, iki operandı toplar ve ilk operanda yazar.
>`add rax,rdx # rax=rax+rdx`

> - sub, ikinci operandı ilkten çıkartır ve ilk operanda yazar
>`sub rsp, 0x10 # rsp=rsp - 0x10 `

>- push , stacke 8 bytes büyür (x86 sistemlerde 4 bytes)
>`push rax # rax içerisindeki değer stack'in en üstünde bulunur.`

> - pop, stackin en üst değerini stackten çıkartır.
>`pop rax`

>- jmp, verilen adrese gider.
>`jump 0x602010 # verilen adrese gider ve oradaki kodu çalıştırır`





#stack
### Stack 
LIFO veri yapısında verilerin tutulduğu ve çıkartıldığı yer. Stack RAM içerisinde bir parçadır. Stack low adrese doğru büyür ve heap tam tersi.

İntel sistemlerde RSP points top of the stack - kullanılan en küçük adres

Stack içerisinde
- return adressler bulunabilir Bİr fonksiyon çağrılıp return ettiğinde tekrar geri dönebilir bulunduğu adrese
- local variables
- pass arguments between funcs
- dinamik olarak tutulan memory blokları alloc(), malloc()

![](pics/Pasted%20image%2020230123112157.png)

Basit bi stack diyagram örneği

![](pics/Pasted%20image%2020230125104845.png)

#heap

hafızanın otomatik olarak yönetilmediği yer. Heap üzerinde yer ayırtmak için malloc(), calloc() gibi metotlar kullanılabilir. İşlem tamamlandıktan sonra kullanılan alanı temizlemek gerekir. Heap içerisinde uzunluk kısıtlaması bulunmaz. Heap memoryden okuması ve oraya yazması biraz yavaştır ve içerisinde bulunanlara herhangi bir fonksiyon erişebilir. 


#vim-editor

![](pics/Pasted%20image%2020230127233921.png)

Her assembly programı içerisinde 3 parçaya bölünmektedir.

1- Data section: bu kısımda tanımlanan değişmeyecek değerler ve veriler-  constantlar bulunur.
2- BSS Section: değeri belirtilmemiş değişkenleri içerir
3- Text Section: kod bölümünün bağşlangıç noktasını kernele söyler.





#arm32 

![](pics/Pasted%20image%2020230128175528.png)

içeriği veya içeriğin adresini registera yükleyebilir (load) veya depolayabilir(store). örneğin,

ldr, r4 \[r10] @ r10 içeriğini r4'e yükler eğer decimal bir değer ise .

str, r9, \[r4] @ r9 içerisindekileri r4 adresine depolar mesela r9= 0x02 ise bu değer r4'un adresinde tutulur.

Not: @ sembolünden sonra yazılanlar yorum olarak kabul edilir.  