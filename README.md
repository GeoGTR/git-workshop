Şirket içi workshop günleri için hazırlanmıştır.

Git Nedir ?
=============

Git, yazılım geliştirme sürecini daha düzenli, takip edilebilir ve işbirlikçi bir şekilde yönetmeyi sağlayan bir versiyon kontrol sistemi aracıdır.

<a href="http://git-scm.com/about">http://git-scm.com/about</a>

Config
-----

Yapılacak ilk şey kimliğinizi oluşturmaktır. Bu, sizi projeyi indiren diğer kişilere tanıtır.

    $ git config --global user.name "Your Name"
    $ git config --global user.email your.email@example.com

Starting 
---------------------

Repoyu clone luyoruz:

    $ git clone https://github.com/GeoGTR/git-workshop.git

Deponuzu klonladıktan sonra, şimdi `git-workshop` adlı bir dizin görmelisiniz. Bu sizin `working directory` nizdir (çalışma dizini) 

    $ cd git-workshop
    $ ls
Workflow
----------------
Yerel depo, git tarafından yönetilen üç "ağaçtan" oluşur. birincisi gerçek dosyaları tutan `working directory`. ikincisi `Stage` ve sonuncusu yaptığınız son commit'i gösteren `HEAD`.
![enter image description here](https://rogerdudler.github.io/git-guide/img/trees.png)
![enter image description here](https://champlintechnologiesllc.com/wp-content/uploads/2017/07/git-basic_600x492.jpg)
![](https://www.bitdegree.org/learn/storage/media/images/dfa373a7-2928-42c6-b031-27f58d6ea8d1.png)

Staging Area
----------------

Şimdi projeye bazı dosyalar eklemeyi deneyelim.

`featureMain.txt` ve `featureTest.txt` adında iki dosya oluşturalım.

    $ touch featureMain.txt featureTest.txt

Dosyaları staging area kısmına ekleyelim 

    $ git add featureMain.txt featureTest.txt
    $ git add . #tüm dosyaları tek seferde eklemek için kullanılır
    
Posta benzetmesi.

Git'te, önce `git add` kullanarak `staging area`'ya içerik eklersiniz. Bu, göndermek istediğiniz şeyleri bir zarfın içine koymak gibidir. İşlemi sonlandırırsınız ve `git commit` kullanarak git dizinine kaydedersiniz. Bu, zarfı mühürlemek gibidir ve artık gönderilmeye hazırdır.

Committing
----------

Artık commiylemeye hazırsınız. `-m` flagıyla commitle alakalı bir mesaj girmenize izin verir.

    $ git commit -m "Iki adet dosya eklendi"

Neler Olduğuna Bakış
----------------------------

Son durum ne bakalım.

    $ git log

Log, geçmişten en yakın zamana kadar listelenen tüm commitleri göstermelidir. Commit sahibinin adı, commit edildiği tarih, commit SHA numarası ve commit mesajı gibi çeşitli bilgiler görürsünüz.

Commit ile alakalı hangi dosyalar ile alakalı işlemler olmuş gibi daha detaylı bilgileri için `git show` kullanabiliriz.

    $ git show

Farklılıklara Bakmak
----------------------

 `featureTest.txt` dosyasının içine birşeyler yazalım:

Dosyayı kaydedelim. 

Şu anda yaptığımız basit bir değişiklik olsada proje büyüdükçe işler karışabilir. Nelerin değiştiğine kolayca bakabilmek için;

    $ git diff

Geri alma
-------

Yaptığımız şeyleri staged area ya aldık ama geri almak istiyoruz.

    $ git reset featureTest.txt
    
 Henüz pushlamadığınız son commit i iptal etmek için

    
    $ git reset --hard HEAD~1
    $ git reset --hard origin/master

Reset
-------
Reset komutunun ilk yaptığı şey HEAD'in konumunu değiştirmektir. HEAD, şu anda bulunduğunuz commit'i gösterir. HEAD~1, HEAD'in bir önceki commit'i gösterdiği anlamına gelir. HEAD~2, HEAD'in iki önceki commit'i gösterdiği anlamına gelir. Bu şekilde devam eder. 

    $ git reset HEAD~1

Resetin ikinci yaptığı şey indexi güncellemektir. Bu verilen flag e göre değişiklik gösterilir. 

![](https://i.stack.imgur.com/iu0at.png)


`--mixed` kullanılırsa working directory sabit kalır, index ve head değişir.

`--soft` kullanılırsa working directory ve index sabit kalır, head değişir.

`--hard` kullanılırsa working directory, index ve head değişir.



Branching
---------

 `git branch` komutu tek başına kullanıldığında branchleri listeler

    $ git branch

`*` hangi branchte olduğunuzu gösterir

Yeni bir branch oluşturmak istiyorsak

    $ git checkout -b feature1
   
Branchi başka bir branchi baz alarak oluşturma

    $ git checkout -b feature1 master

Branch değiştirmek için

    $ git checkout (branch name)

Farklı bir branchteyken birşeyler ekleyelim

    $ echo 'some content' > feature1.txt
    $ git add feature1.txt
    $ git commit -m "Added experimental feature1.txt"

İki branch arasındaki farka bakalım `git diff`

    $ git diff master


Merging
-------

Bu yeni branchimiz bizim geliştirdiğimiz bir özelliği barındırdığını varsayalım. Geliştirme sürecimiz bitti ve ana branche bu özellikleri eklemek istiyoruz. Burada `merge` kavramı devreye girmiş oluyor

İlk önce hangi branchin içine mergeleyeceksek o branche geçiş yapıyoruz

    $ git checkout master

Mergeleyeceğimiz branchin adıyla birlikte merge komutunu çağırıyoruz.

    $ git merge feature1


Fixing Conflicts and Merge 
---------------

Git, aynı dosya düzenlendiğinde bile otomatik olarak birleştirme konusunda oldukça iyidir. Bununla birlikte, aynı kod satırının düzenlendiği bazı durumlar vardır ve bir bilgisayarın nasıl birleştirileceğini anlayamayabilir. Bu, düzeltmeniz gereken bir conflicti tetikleyecektir.

Yeni branch oluşturalım

    $ git checkout -b feature2

Bu branchte `featureMain.txt` dosyasını değiştirip commitliyelim

Tekrar ana branche dönüp aynı dosyayı başka birşekilde değiştirip yine commitliyelim

Sonrasında mergelemeyi denediğimizde sorun yaşadığımızı görüntüleyeceğiz.

    Auto-merging featureMain.txt
    CONFLICT (content): Merge conflict in featureMain.txt
    Automatic merge failed; fix conflicts and then commit the result.

`featureMain.txt` Dosyasını açtığımızda şunun gibi birşey karşımıza çıkacaktır:

    $ cat featureMain.txt
    <<<<<<< HEAD
    featureMain - master branch codes
    =======
    featureMain - feature2 branch codes
    >>>>>>> feature2


Git, standart bir conflict çözme işaretçileri kullanır. `<<<<<< HEAD` ve `======` arasındaki her şey olan bloğun üst kısmı, mevcut branchteki olan kısımdır. Alt yarı, `feature2` dalından gelen sürümdür. Çatışmayı çözmek için ya bir tarafı seçersiniz ya da uygun gördüğünüz gibi birleştirirsiniz.

İşimiz bittiğinde merge i tamamlamak için normal commit adımları tekrarlanır.
`git add` and `git commit`.

    $ git add feature2.txt
    $ git commit -m "Fixed conflict"
