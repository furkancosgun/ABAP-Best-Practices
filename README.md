
# ABAP En İyi Pratikler

Temiz ABAP geliştirmenin ortak ilkeleri.

### Contributing

Bir şey mi eksik? Bir şey mi yanlış? Bir yazım hatası mı buldunuz?
Bir  issue veya pull request bulunmaktan çekinmeyin.

#### İçerikler

- Stil ve Yönergeler 
    - Pretty-Printer
    - Adlandırma Kuralları
    - snake_case
    - Tutarlı yazım
    - Eski ifadelerden kaçının
- Kodlama
    - Endişelerin Ayrılığı ilkesi
    - Kendini tanımlayan kod
    - Nasıl yaptığınızı değil, ne yaptığınızı yorumlayın
    - Mümkün olduğunca yerel calisin
    - Degisebilir sayılar kullanmayın
    - Derin yuvalamadan kaçının
    - Otomatik kod kontrollerini kullanın
    - Ölü Kodu silin
    - Hataları görmezden gelme
    - Sınıf tabanlı istisnaları kullanır
    - İstisnaları mümkün olan en kısa sürede ele alır
    - Bir problem başına bir istisna sınıfı, farklı problem sebepleri için birkaç metin
    - Program erişilebilirliğini kontrol edin
    - Herhangi bir paylaşılan veri erişimini veri erişim sınıflarına sarın
    - Örtük veri bildirimlerinden kaçının
    - Yerleşik Boolean türlerini ve sabitlerini kullanın
    - Kullanıcı arayüzünde sistem alanlarını kullanmayın
    - Uygun bir dahili tablo kategorisi kullanın
    - ir tablo satırına erişmek için uygun yolu seçin
    - Bir döngüdeki tabloyu modify etmeyin
    - RETURN ile işlemden çıkın
    - Diyalog modüllerinde ve event bloklarında mantık uygulamayın
    - Makroları yalnızca istisnai durumlarda kullanın
    - Örtülü olarak çağrılan mesajlar için anchorlar oluşturun
- Dilve Çeviri
    - Sabit olarak kodlama yapmayın
    - Sabit metinler kullanmayın
    - Metin depolamak için veritabanı metin tablolarını kullanın
    - Bir projedeki tüm nesneler için aynı orijinal dili kullanın
    - Çeviriyi aklınızda bulundurun
    - Geliştirme nesneleri için yalnızca İngilizce adlandırma kullanın
    - Kullanıcı arayüzünde yalnızca çevrilebilir metinleri kullanın
    - Mesajlarda numaralı yer tutucuları kullanın
- Nesne tabanlı programlama
    - İşlevsel modüller yerine sınıfları kullanın veya mümkünse gerçekleştirir
    - SOLID prensiplerini uygula
    - GRASP kullan
    - OOP Tasarim desenlerini ogren
    - Demeter Yasasına Saygı Duyun
    - Yardımcılar, yardımcı programlar vb. sınıflardan kaçının
- Veritabani Kullanimi
    - Mümkünse OpenSQL kullanın
    - Veritabani işlemlerinden sonra sy-subrc'yi kontrol edin
    - Yalnızca ihtiyacınız olan alanları okuyun
    - ForAllEntries icin tabloyu kontrol edin
- Performans
    - Döngülerde SELECT yapmayın
    - FAE ve RANGE yerine JOIN'i tercih edin
    - HANA d FAE Kullanin
    - Veritabani isteklerinin sayısını azaltın
    - Kodunuzu profilleyin
- Test
  - Yalnızca genel arayüzü test edin
  - Testlerinizi izole edin
  - Testleri tekrarlanabilir tutun
  - Birim testlerini davranış örneği ve dokümantasyon olarak kullanın
  - Mimariyi tasarlarken testleri aklınızda bulundurun

    

## Stil ve Yönergeler

  Temiz ve hoş kod yazmak için temel yaklaşımlar

### Pretty-Printer

Pretty-Printer'ı ayarlardan kurun ve kodunuzu her kaydettiğinizde çalıştırın.
  
Aynı sistemlerde farklı biçimlendirmeleri önlemek için proje yönergelerinizde aynı Pretty-Printer ayarlarını yapın.

### Adlandırma Kurallarını Kullan

Oluşturduğunuz her kod veya sözlük nesnesi için adlandırma kuralları seçin. Karışıklığı önlemek için kullanın.

Ayrıca, adlandırma kuralını kontrol etmek için kod denetlemesi ayarlayabilirsiniz.

### snake_case Kullanin

Değişkenlerinizi, yöntemlerinizi, sınıflarınızı vb. kelimelerin arasına `lo_table_container->get_sorted_table_data()` gibi alt çizgiler koyarak adlandırın. ABAP için standart sözleşmedir.

[Wikipedia](https://en.wikipedia.org/wiki/Snake_case#Examples_of_languages_that_use_snake_case_as_convention)

### Tutarlı Yazım Kullanın

ABAP'de (`=`, `<>`, `>=`, vb.) ve (`EQ`, `NE`, `GE` vb.), veri bildirimleri kümesi gibi birçok alternatif dil yapısı vardır. 

Alternatiflerden birini seçin ve geliştirmeniz sırasında tutarlı bir şekilde kullanın.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenalternative_langu_guidl.htm)

### Eski İfadelerden Kaçının

ABAP'taki bazı ifadeler güncelliğini yitirmiştir. Bazıları kullanımdan kaldırıldı, bazıları sadece yeni işlenenlerle değiştirildi. Daha yeni bir alternatif varsa, eski ifadelerden kaçınmaya çalışın.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenmodern_abap_guidl.htm)

## Kodlama

ABAP'da daha iyi kod yazmanın ortak kuralları

### Separation of Concerns İlkesini Uygulayın

Bireysel birimlerin işlevleri arasında minimum örtüşme ile programı birimlere ayırın.

### Kendini Tanımlayan Kod Yaz

İyi kod kendini açıklamalıdır. Kendi kendini tanımlayan kod, çok fazla yorum veya büyük dokümantasyon gerektirmez.

İfadelerinizi açıklayıcı hale getirin, örneğin:

- değişkenler, işlevler, sınıflar vb. için anlamlı adlar seçin (`lv_i` → `lv_index`, `lt_data` → `lt_sale_orders`, `cl_util` → `cl_file_io_helper`, `start()` → `run_accounting_document_processing()`)
- mantıksal adımları yöntemlere ayırın (örneğin, `process_document()` yöntemini `prepare_document_data()`, `is_doc_creation_possible()`, `lock_tables()`, `create_document()`, `unlock_table()`, vb. .)
- bir programlama satır miktarını azaltın
- yuvalama seviyesini azalt
- örtüklükten kaçının

### Nasıl Yaptığınızı Değil, Ne Yaptığınızı Yorumlayın

Uygulamanın özelliklerini yorum yapmayın, kendi kendini tanımlayan bir kod ,bu bilgiyi bir okuyucu için sağlayacaktır.

En iyi durumda, yöntem başlığında veya yöntem çağrısından önce iş mantığı biriminin kısa açıklaması yeterli olacaktır.

### Mümkün Olduğunca Yerel Olun

Mümkün olan en düşük kapsamda değişkenler, yöntemler ve nitelikler oluşturun. Değişkeninizin/yönteminizin kapsamı ne kadar büyükse, programınız o kadar bağlantılıdır.

### Degisebilir Sayılar Kullanmayın

Sabit kodlanmış metinlerden veya adsız değişkenlerden kaçının.

Bunun yerine, onları anlamlı değişkenlere veya sabitlere taşıyın. Aynı ada sahip (`ABC123` ↛ `lc_abc123`) sadece metin sabitini taşımanın yeterli olmadığını unutmayın, ona uygun bir açıklama verin (`ABC123` → `lc_storage_class`)

Kötu:
```abap
lo_doc_processor->change_document(
  iv_blart = 'AB'
  iv_bukrs = 'C123'
  iv_popup = lv_x
).
```

İyi:
```abap
CONSTANTS:
  lc_clearing_document TYPE blart VALUE 'AB',
  lc_main_company      TYPE bukrs VALUE 'C123'.
DATA:
  lv_show_popup TYPE abap_bool.
*...
lo_doc_processor->change_document(
  iv_blart = lc_clearing_document
  iv_bukrs = lc_main_company
  iv_popup = lv_show_popup
).
```

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenliterals_guidl.htm)

### Derin Yuvalamadan Kaçının

Derin iç içe döngüler, switch case ve diğer kontrol yapıları yazmayın. Kontrol yapısından çıkışları if'ler ile, kontroller ve dönüşlerle iç içe yerleştirmeyin.

Kötu:
```abap
LOOP AT lt_data ASSIGNING <ls_data>.
  IF a = b.
    IF c = d.
      IF ls_data IS NOT INITIAL.
        ls_data-field = 'aaa'.
      ENDIF.
    ELSE.
      ls_data-field = 'bbb'.
    ENDIF.
  ELSE.
    ls_data-field = 'ccc'.
  ENDIF.
ENDLOOP.
```

İyi:
```abap
LOOP AT lt_data ASSIGNING <ls_data>.
  IF a <> b.
    ls_data-field = 'ccc'.
    CONTINUE.
  ENDIF.

  IF c <> d.
    ls_data-field = 'bbb'.
    CONTINUE.
  ENDIF.

  CHECK ls_data IS NOT INITIAL.
  ls_data-field = 'aaa'.
ENDLOOP.
```

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abennesting_depth_guidl.htm)


### Otomatik Kod Kontrollerini Kullanın

Kod sözdiziminizi, mimarinizi, yönergelerinizi, güvenlik açıklarını ve diğer kalite özelliklerini doğrulamak için sözdizimi denetimi, genişletilmiş program denetimi ve kod denetçisini kullanın.

[Open Source list of checks for SCI/ATC](https://github.com/larshp/abapOpenChecks)

### Ölü Kodu Sil

Eski ve kullanılmayan kodu kaldırın. Sözdizimi denetimi ve genişletilmiş program denetimi, onu bulmanıza yardımcı olacaktır.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abendead_code_guidl.htm)

### Hataları Görmezden Gelme

Hatalara tepki verin. Bu, uygun bir mesaj, bir blog girişi veya bir istisna oluşturma olabilir. Ama onları görmezden gelmeyin, aksi takdirde herhangi bir sorunun nedenini bulmanız mümkün olmayacaktır.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenreaction_error_guidl.htm)

### Sınıf Tabanlı İstisnaları Kullanın

ABAP'da birkaç tarihsel hata türü vardır - sistem istisnaları, klasik istisnalar ve sınıf tabanlı istisnalar. Sistem istisnalarının kullanımına izin verilmez, klasik istisnalar açık ve güncel değildir. Sınıf tabanlı istisnaları kullanmak için hiçbir neden yoktur.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenclass_exception_guidl.htm)

### İstisnaları Mümkün Olan En Kısa Sürede Ele Alın

İstisnayı, çağrı yığınında yükseltme maddesine daha yakın ele almak daha iyidir. Geçerli yığın düzeyinin bağlamı, uygun kullanım için yeterli bilgiye sahip olduğunda bunu ele alın.


### Bir Problem Başına Bir İstisna Sınıfı, Farklı Problem Sebepleri İçin Birkaç Metin

Her yükseltme maddesi için çok farklı sınıflar oluşturmayın. Bunun yerine bir tür problem için bir sınıf oluşturun, yani bir tür problem işleme için.

Bu tür bir sorunun nedenini açıklayan anlamlı mesajlar oluşturun. Hata işleme aynı olabilir, ancak nedenler, günlük kaydı ve kullanıcı bildirimi farklı olacaktır.

Örnek: PC'den bir dosya okuyorsunuz. Farklı bir sorun olabilir - dosya bozuk, dosya boş, erişim reddedildi vb. Ama sadece bilmek istiyorsanız, dosyanın başarılı bir şekilde yüklenip yüklenmediğini bilmek istiyorsanız, bir istisna sınıfı `zcl_io_error` yeterli olacaktır. Ancak, kullanıcıya dosyanın tam olarak *neden* yüklenmediğini bildirmek için her bir hata nedeni veya hata türü için uygun mesajlar oluşturun.

### Program Erişilebilirliğini Kontrol Edin

Projenizin engelli kişiler tarafından kullanılabildiğinden emin olun. Bu, kullanıcı arayüzündeki herhangi bir bilginin erişilebilir bir biçimde verilmesi gerektiği anlamına gelir:

- giriş ve çıkış alanları anlamlı etiketlere sahip olmalıdır;
- simgeler bir araç ipucuna sahip olmalıdır;
- tablo sütunlarının bir başlığı olmalıdır;
- bilgiler yalnızca renkle ifade edilmemelidir;
- ekrandaki giriş ve çıkış alanları, her biri anlamlı bir başlık ile çerçeveler halinde uygun şekilde gruplandırılmalıdır.

Bu, renk körlüğü veya ekran okuyucu kullanıcıları gibi engelli kişilerin uygulama işlevlerine tam erişime sahip olmasını sağlar.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenaccessibility_guidl.htm)

### Herhangi Bir Paylaşılan Veri Erişimini Veri Erişim Sınıflarına Sarın

Paylaşılan bellek, paylaşılan nesneler, arabellekler vb. gibi bazı paylaşılan verileri kullandığınızda, bunlara doğrudan erişmeyin. Bunun yerine, bunları statik veri erişim sınıfının ayarlayıcı ve alıcı yöntemlerine sarın.


### Örtük Veri Bildirimlerinden Kaçının

Mümkün olduğunda `TABELS`,`NODES` gibi veri bildirimlerini kullanmamaya çalışın. Örtük erişime sahip veri nesneleri oluştururlar.

Bunun yerine `DATA`yı kullanın. LDB ile yalnızca `NODES` kullanın. Her ikisi de tüm program boyunca paylaşılacak ve kullanılacak küresel çalışma alanları yaratır.

Asla `TABEL ... WITH HEADER LINE` kullanmayın. Tablo satırı türü veya satır içi bildirimler ve tablo ifadeleri ile `structre`,` field-symbol` veya `ref` kullanın.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abentable_work_area_guidl.htm)

### Yerleşik Boolean Türlerini Ve Sabitlerini Kullanın

Bazı mantıksal bilgileri kullanmak istediğinizde, `char1` veya diğer türler yerine yerleşik `Boolean` türü `abap_bool` kullanın

Boolean true ve false değerleri için `abap_true` ve `abap_false` sabitlerini kullanın. `X`, ` `gibi sabitler kullanmayın - bu bir sabit koddur.

`space`,`IS INITIAL` veya `IS NOT INITIAL'`kullanımı da tavsiye edilmez, çünkü bunlar `abap_bool`un teknik uygulamasının durumunu kontrol eder, ancak gerçek Boolean veri nesnesinin anlamını değil.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abendataobjects_true_value_guidl.htm)

### Kullanıcı Arayüzünde Sistem Alanlarını Kullanmayın

Sistem alanları (sy) tekniktir. Kullanıcıya gösterilmemelidirler.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenuse_ui_guidl.htm)

### Uygun Bir Dahili Tablo Kategorisi Kullanın

Uygun bir tablo kategorisi seçin. Küçük tablolar için dizinler veya karma anahtarlar gereksiz olabilir, ancak büyük tablolar için her zaman aşağıdaki kuralı kullanın:

- index erişimleri: standart tablo
- index erişimleri ve key erişimleri: sorted tablo
- yalnızca key erişimleri: hashed tablolar

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenselect_table_type_guidl.htm)

### Bir Tablo Satırına Erişmek İçin Uygun Bir Yol Seçin

Okurken dahili bir tablonun erişilen satırını saklamanın üç yolu vardır - `INTO` satırı yapıya kopyalar, `ASSIGNING` satırı field-symbole atar ve `REFERENCE INTO` satıra bir referans oluşturur. Aynısı tablo ifadeleri için de geçerlidir, ancak sonucun kategorisine göre bir satır depolama türü seçilir.


Kural:

- satır türü darsa ve okuma satırı değiştirilmeyecekse bir çalışma alanı (structre) kullanın.
- satır tipi geniş veya derinse ve okunan satır değiştirilecekse field-symbol kullanın.
- satır tipi geniş veya derinse ve okuma satırına bir referans geçirilecekse reference kullanın.

Performans açısından, döngülerde satır kopyalamaktan kaçınmak daha iyidir.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abentable_output_guidl.htm)

### Döngüdeki Tüm Tabloyu Modify Etmeyin 

Dahili bir tabloda döngü yaparken, tüm tablo gövdesini değiştirecek ifadeleri çalıştırmayın. Tabloyu yalnızca satır satır `modify` edin.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenloop_guidl.htm)

### RETURN ile İşlemden Çıkın

Yöntemden, işlevden, formdan vb. çıkmak için yalnızca `RETURN` kullanın. `CHECK` veya `EXIT` kullanmayın.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenexit_procedure_guidl.htm)

### Diyalog Modüllerinde ve Event Bloklarında İşlev Gerçekleştirmeyin 

Bunun yerine, işlev uygulamasını kapsayan ilgili sınıf yöntemini çağırın.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abendial_mod_event_block_guidl.htm)

### Makroları Yalnızca İstisnai Durumlarda Kullanın


Mümkün olduğunda makro kullanımından kaçının. 
Makronun birkaç dezavantajı vardır:

 - hata ayıklayamaz;
 - sözdizimi denetimi yok; 
 - örtük çağrı arayüzü 
 - arayüz parametresi tipi kontrolü yok.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenmacros_guidl.htm)

### Örtülü Olarak Çağrılan Mesajlar İçin Sabitler Oluşturun

Mesaj niteliklerini (sınıf, sayı, argümanlar) örtük olarak ilettiğinizde, örn. işlev veya yöntem çağrısı ile veya değişkenler kullanarak, mesajın kullanıldığı yer listesinde aranabilir olmasını sağlamak için sabitler kullanın.


Aşağıdaki şekilde yapılabilir (bir standartta yapıldığı gibi):
```abap
IF 1 = 2. MESSAGE i123(abc) ... . ENDIF.
```

or 

```abap
MESSAGE i123(abc) ... INTO sy-msgli.   
"ve ardından mesaj özniteliklerini iletmek için sy-msg* alanlarını kullanın
```

## Dil ve Çeviri

Program nasıl uluslararasılaştırma ve yerelleştirmeye hazır hale getirilir

### Metinleri Sabit Kodlamayın

Metinleri asla satır içi metin sabitleri olarak yazmayın - bulmak zordur ve tercüme etmek mümkün değildir. Bunun yerine mesaj sınıfını veya text sembolünü kullanın. 

### Metin Sabitlerini Kullanmayın

Metni depolamak için sabitler kullanmayın. Metin sabitleri çevrilemez. Bunun yerine mesaj sınıfını veya metin sembolünü kullanın.

### Veritabanın'da Metin Depolamak İçin Metin Tablolarını Kullanın

Metinleri diğer verilerle aynı sözlük tablolarında saklamayın. Metin tabloları oluşturun ve bunları bir yabancı anahtar aracılığıyla ana tabloya atayın. Metin tablolarının bazı avantajları vardır:

- Çeviriyi desteklerler;
- Aynı nesne için birkaç metin işlenebilir (örn. kısa, orta, uzun metin, vb.);
- Arama yardımına gerek yok. Metin tablosundaki metinler otomatik olarak bir değer listesine eklenecektir;
- Ekstra bakım gerektirmez. Metin sütunu, bir bakım görünümüne otomatik olarak eklenecektir;
- `SE63` işlemi ile çeviri yapılabilir.

### Bir Projedeki Tüm Nesneler İçin Aynı Orijinal Dili Kullanın

Bir dil seçin ve yeni nesneler oluşturduğunuzda onu kaynak olarak kullanın. Gelecekte bakımı ve tercümesi daha kolay olacaktır.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenoriginal_langu_guidl.htm)

### Çeviriyi Aklınızda Bulundurun

Tüm metinlerin, mesajların, adların vb. çevrilebileceğini unutmayın. Farklı dillerde ifadelerin farklı uzunlukları (hatta farklı gerekçeleri) olacaktır. Bunun için biraz boş alan bırakın.

*İngilizce ifadeler, diğer en çok kullanılan dillerden çok daha kısadır.*

### Geliştirme Nesneleri İçin Yalnızca İngilizce Adlandırma Kullanın


Değişkenler, yöntemler veya sınıf adları gibi bazı programlama nesnelerini veya türler, yapılar, tablolar vb. gibi sözlük nesnelerini adlandırırken yalnızca İngilizce adları kullanın.

Başka diller kullanmayın, birleştirmeyin. İngilizce çoğu ülkede anlaşılabilir, kodunuzu uluslararası hale getirmek faydalı ve kibar. Belki başka bir ülkeden başka bir ekip tarafından desteklenir.

Ve bu, programlama dünyasında sadece bir standart ve en iyi uygulamadır. Barbar olmayın.


### Kullanıcı Arayüzünde Yalnızca Çevrilebilir Metinleri Kullanın

Mesajlar, OTR, metin sembolleri vb. gibi yalnızca çevrilebilir metinleri kullanıcıya gönderin

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abensystem_text_guidl.htm)

### Mesajlarda Numaralı Yer Tutucuları Kullanın

Anonim yer tutucular `&` yerine numaralı `&1` - `&4` yer tutucularını kullanın. Girilen sözcüklerin sırası farklı dillerde farklılık gösterebilir. Bir çevirmenin mesaj metinlerini çevirirken değiştirilen metinlerin sırasını değiştirmesi gerekebilir. Anonim yer tutucularla bu mümkün değildir.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abentrans_relevant_text_guidl.htm)

## Nesne Tabanlı Programlama

Burada, yalnızca SAP'ye özgü değil, aynı zamanda yaygın uygulamalar da dahil olmak üzere bazı OOP en iyi uygulamaları verilmiştir.


### İşlevsel Modüller Yerine Sınıfları Kullanın veya Mümkünse Gerçekleştirir


*SAP, nesne yönelimli olmayan kod modüllerinin kullanımının artık geçerli olmadığını varsayıyor.*

FM'leri yalnızca sınıfları kullanma imkanı olmayan yerlerde kullanın (örneğin, RFC, güncelleme modülleri, vb.)

ABAP bir kurumsal programlama dilidir ve OOP, diğerlerinin karmaşık tanımlamalarından daha iyi olabilir.


Ayrıca, tüm yeni SAP teknolojileri sınıf tabanlıdır.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenabap_obj_progr_model_guidl.htm)

### SOLID Prensiplerini Uygula

OOP geliştirmede SOLID prensiplerini kullanın. İşte esnek ve genişletilebilir bir yazılım geliştirmenin beş temel ilkesi:

- [**S**ingle responsibility principle](https://en.wikipedia.org/wiki/Single_responsibility_principle)
- [**O**pen/closed principle](https://en.wikipedia.org/wiki/Open/closed_principle)
- [**L**iskov substitution principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle)
- [**I**nterface segregation principle](https://en.wikipedia.org/wiki/Interface_segregation_principle)
- [**D**ependency inversion principle](https://en.wikipedia.org/wiki/Dependency_inversion_principle)

### GRASP Kullan

Bu, nesne yönelimli tasarımda sınıflara ve nesnelere sorumluluk atamak için bir dizi kalıp ve ilkedir. 

Yalnızca yararlı davranış kalıpları değil, aynı zamanda "düşük eşleşme" ve "yüksek uyum" gibi çok önemli ilkeler de vardır.

[Wikipedia](https://en.wikipedia.org/wiki/GRASP_(object-oriented_design))

### OOP Tasarim Desenleri Ogren

Kurumsal geliştirmede çok kullanışlı olan bir dizi iyi bilinen klasik OOP tasarım deseni vardır. Bunları biliyorsanız, tasarım fikirlerini ekiple kolayca paylaşabilir, mimari zorlukları daha hızlı çözebilir ve daha iyi sorun çözümleri bulabilirsiniz.


### Demeter Yasasına Saygı

- her birim diğer birimler hakkında sadece sınırlı bilgiye sahip olmalıdır: sadece mevcut birime "yakından" ilgili birimler; 

- her birim sadece arkadaşlarıyla konuşmalıdır; yabancılarla konuşmayın; 
- sadece yakın arkadaşlarınızla konuşun.

Bu, A sınıfının B sınıfına erişimi varsa ve B'nin de C sınıfına erişimi varsa, A sınıfının doğrudan `A->B->C->method_of_C( gibi bir C yöntemini çağıramaması gerektiği anlamına gelir. )`. B'nin bunun için özel bir yöntemi olması gerekir.

Örneğin, bir köpeğin havlamasını istiyorsak, `lc_dog->get_head()->get_voice_functions()->run_bark_sound()` değil `lc_dog->bark()` çağıracağız."

[Wikipedia](https://en.wikipedia.org/wiki/Law_of_Demeter)

### Yardımcılar, Yardımcı Programlar vb.  Sınıflardan Kaçının.


ABAP geliştirmede bazı yardımcı yöntemlere ihtiyaç duyulabilecek birçok durum vardır.
Ancak statik yöntemler yerine onları güçlü nesnelere dönüştürün. 
İşlevi, bu tür işlemleri yapmaktan sorumlu bir sınıf olarak düşünün. 
Örneğin. Excel tablosunu dize tablosuna yüklemek için yardımcı program yerine, yapıcıda bir Excel tablosu yükleyen ve istediğiniz herhangi bir şekilde (hatta bir dize tablosu olarak) size verebilen bir sınıf oluşturun.

Devam edebilir ve tablo verisi biçimlendirmesi için bir arabirim oluşturabilir ve bunu çeşitli tablolar için uygulayabilirsiniz - Excel, CSV, XML, vb. Veya bir tablo okuyucu arabirimini tablo biçimlendiricinin yapıcı parametresi olarak ayarlayabilir ve farklı türler için uygulayabilirsiniz. veri yüklemeleri.
Her durumda, sadece statik bir yöntem olarak daha esnek ve daha kullanışlı olacaktır.

Örnek:

- `lt_str = cl_file_util=>upload_file_into_str_tab( lv_path )` → `lt_str = NEW cl_file( path )->get_str_tab( )`
 
- `lv_date = cl_format_util=>format_date_to_gmt( sy-datum )` → `lv_date = NEW cl_date_formatter( sy-datum )->get_gmt( )`

## Veritabanı Kullan

Verimli DB istekleri nasıl oluşturulur?

### Mümkünse OpenSQL Kullanın

Yerel SQL yerine OpenSQL (7.53'ten beri ABAP SQL) kullanın.
Birkaç avantajı vardır:

- sözdizimi kontrolü;
- doğrulama kontrolü;
- sunucular arası yorumlama;
- ortak sözdizimi;
- ABAP ile entegrasyon.

NativeSQL kullanımının ana nedenleri: performans sorunları, belirli DB işlevleri.

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abendatabase_access_guidl.htm)

### Veritabani İşlemlerinden Sonra sy-subrc  Kontrol Et

`sy-subrc`'yi kontrol ederek Veritabani işlem durumunu açıkça kontrol edin.

Herhangi bir hata işlemeniz olmasa bile, bunu açık hale getirmek için `sy-subrc` işaretini koyun ve hata işlemenin gerekmediğini herkesin bilmesini sağlayın.

```abap
IF sy-subrc <> 0. 
  * to do
ENDIF 
```

[SAP Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenreturn_value_guidl.htm)

### Yalnızca İhtiyacınız Olan Alanları Okuyun

Kodunuzda `SELECT *` kullanmaktan kaçının. Birkaç sebep var:

- Ne kadar çok alan alırsanız, o kadar çok zaman alır ve o kadar çok Veritabani kanal kapasitesi kullanır. Gereksiz alanların okunması performans nedenlerinden dolayı kötüdür;

- Veritabanı tablosu şeması gelecekte değişebilir. Programınızı uyarlamayı unutursanız (ki muhtemelen yapacaksınız, çünkü tablo şeması başka bir kişi tarafından değiştirilebilir), programınız için gerekli olmayan alanları okuyacaksınız.

İstisna: benzersiz ve belirli bir kullanım durumu için tasarlanmış tüketim CDS görünümleri, bu nedenle muhtemelen yalnızca gerekli alanlardan oluşacaktır.

### FAE Tablo Boşlugunu Kontrol Et


`FOR ALL ENTRIES` deyiminde kullanmak üzere olduğunuz tablonun boş olup olmadığını daima kontrol edin.

Aksi takdirde, `SELECT` ,  `WHERE` koşullarını yok sayarak veritabanı tablosunun içerdiği **Her** girişi döndürür.

Yani veritabanı tablonuz 1K girdi içeriyorsa ve `WHERE` yan tümceniz sayıyı 10'a indiriyorsa, FAE'deki tablo boşsa `SELECT` 1K girdi getirecektir.

## Performans

Performans darboğazlarını önlemek için bu kuralları kullanın

### Döngülerde SELECT Yapmayın

`DO-ENDDO`, `WHILE-ENDWHILE`, `LOOP-ENDLOOP`, `PROVIDE-END PROVIDE` ve `SELECT-ENDSELECT` gibi döngülerde Veritabanı işlemlerinden kaçınmaya çalışın.
  
Bunun yerine, işlem kriterlerini döngüden çıkarın ve Veritabanı işlemini **bir kez** yürütün.

DB işlemlerini döngülerden ayırmak mimari açıdan zor olabilir. Bu sorunu çözmek için, tüm DB işlemlerini bazı sınıflara (veya sınıflar grubuna) yerleştirmek için Veri Erişim Sınıfı (DAC) modelini kullanabilir ve DB işlemlerini mümkün olduğunca az gerçekleştirebilirsiniz (örneğin, temel yük kullanarak döngüden önce veya döngüden sonra). Ardından, bir döngüde, ancak Veritanaın'dan değil, DAC'de kapsüllenmiş dahili tablodan düzenli bir okuma gerçekleştirin.

### FAE ve RANGE Yerine JOIN'i Tercih Edin

Birleştirme işlemleri çok daha hızlıdır çünkü çok fazla girdileri yoktur (Uygulamadan Veritanı'na ek transfer yoktur) ve alan eşleme (`SELECT`'inizin `ON` kısmı) dahili Veritabanı yapıları kullanılarak yapılır (ek dönüştürme gerekmez) .


### HANA'da FAE Kullanın

FAE (`FOR ALL ENTRIES`) HANA'da hala geçerlidir. DB'nizi mevcut en son yama düzeyine güncellediğinizden ve FDA'yı (FAST DATA ACCESS) kullandığınızdan emin olun. FDA işlemleri, geleneksel FAE'den 10 kat ila 100 kat daha hızlıdır.

[2399993 - FAQ: SAP HANA Fast Data Access (FDA)](https://launchpad.support.sap.com/#/notes/2399993)

### Veritabanı İsteklerinin Sayısını Azaltın

Kodunuzu mümkün olduğunca az Veritabanı isteği yürütecek şekilde düzenlemeye çalışın. Veritabnı erişimi genellikle bir darboğazdır ve bir dizi Veritabanı oturumu sınırlıdır. Performansı göz önünde bulundurmaya çalışın ve benzer istekleri birlikte gruplayın.

Her satır için 10 istek yerine 10 satırı bir kez getirin. Sonuçları gerçekten kullanmadan önce, örneğin istek anahtarlarını hesaplamak için bir program mimarisini değiştirmeniz gerekebilir.

### Kodunuzu Profilleyin

Liste raporundan daha karmaşık bir şey yazarken kodunuzun profilini çıkarın. Kodunuzda herhangi bir darboğaz ve performans sorunu olmadığından emin olmak için `ST05`, `SAT` veya ihtiyacınız olan diğer herhangi bir profil oluşturma aracını kullanın.

## Test
  
Kod kalitenizi nasıl kontrol edebilirsiniz?

### Yalnızca Genel Arayüzü Test Edin

Birim testinde, sınıfın herhangi bir dahili uygulamasını test etmemelisiniz, örn. özel ve korumalı yöntemler.

Yalnızca genel yöntemleri test edin, programın sınıfı "gerçek hayatta" kullanma şeklini simüle edecektir. Her dahili yöntem, yine de genel yöntemlerden çağrılacak. Değilse - yöntem sınıfta gerçekten kullanılmaz ve kaldırılmalıdır.

Test, uygulamasını değil, sınıfın davranışını kontrol etmelidir. Uygulamayı yeniden düzenleyebilir veya değiştirebilirsiniz, ancak test aynı olacaktır. Sınıfın davranışı değişmeyecekse, testte de değişiklik yapılması gerekmez.

Yalnızca genel yöntemleri test ederek herhangi bir dahili mantığı kapsüllenmiş halde bırakın.

### Testlerinizi İzole Edin

Testler birbirini etkilememelidir. Her test, yalıtılmış ortamlarda ayrı ayrı çalıştırılmalıdır - ör. bir test çalıştırmasından önce/sonra ortamı temizlemelisiniz.

### Testleri Tekrarlanabilir Tutun

Aynı test girdisi aynı çıktıyı vermeli, gerçek değerler beklentileri karşılamalı, test davranışı tekrarlanabilir olmalıdır.

### Birim Testlerini Davranış Örneği ve Dokümantasyon Olarak Kullanın

Birim testi, bir program modülünün nasıl çalışması gerektiğini gösterir. Gerçek bir programda kullanıldığı gibi aynı şekilde test edecekseniz, bir geliştirici için en iyi dokümantasyon (örneklerle!) olacaktır.

### Mimariyi Tasarlarken Testleri Aklınızda Bulundurun

Yalnızca iyi tasarlanmış programlar kolayca test edilebilir. Birim testini kolaylaştırmak için modüllerinizin bağımsız olarak test edilmesini sağlayın. Bunları düşük bağlantılı ve son derece uyumlu hale getirin, gereksiz ilişkileri kaldırın, bağımlılık enjeksiyonunu kullanın, testlerde rahat etmek için bağımlı sınıfları arayüzlerin arkasına gizleyin. SoC, SOLID, GRASP sizin arkadaşlarınızdır.

