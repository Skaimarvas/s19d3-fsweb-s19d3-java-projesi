# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

# Proje Kurulumu
Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

## Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#Tablolar 
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]



# Görevler
Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın. 



	1) ÖRNEK SORU: Yazar tablosunu KEMAL UYUMAZ isimli yazarı ekleyin.
	
    INSERT INTO "kütüphane".yazar
    VALUES (DEFAULT,'KEMAL','SAYAR')
	
	2) Biyografi türünü tür tablosuna ekleyiniz.

    INSERT INTO "kütüphane".tur
    VALUES (DEFAULT,'Biyografi');
	
	
	3) 10A sınıfı olan ÇAĞLAR ÜZÜMCÜ isimli erkek, sınıfı 9B olan LEYLA ALAGÖZ isimli kız ve sınıfı 11C olan Ayşe Bektaş isimli kız öğrencileri tek sorguda ekleyin. 
	
    INSERT INTO "kütüphane".ogrenci
    VALUES (DEFAULT,'ÇAĞLAR','ÜZÜMCÜ','01-01-1992','Erkek','10A','75'),
    (DEFAULT,'LEYLA','ALAGÖZ','02-01-1992','Kız','9B','85'),
    (DEFAULT,'AYŞE','BEKTAŞ','03-01-1992','Kız','11C','95');
	
	4) Öğrenci tablosundaki rastgele bir öğrenciyi yazarlar tablosuna yazar olarak ekleyiniz.

    INSERT INTO "kütüphane".yazar (ad,soyad)
    SELECT ad, soyad FROM "kütüphane".ogrenci ORDER BY RANDOM() LIMIT 1;
	
	5) Öğrenci numarası 10 ile 30 arasındaki öğrencileri yazar olarak ekleyiniz.

    INSERT INTO "kütüphane".yazar (ad,soyad)
    SELECT ad, soyad FROM "kütüphane".ogrenci WHERE ogrno BETWEEN 10 AND 30;	
	

	6) Nurettin Belek isimli yazarı ekleyip yazar numarasını yazdırınız.
	(Not: Otomatik arttırmada son arttırılan değer @@IDENTITY değişkeni içinde tutulur.)

    INSERT INTO "kütüphane".yazar
    VALUES (DEFAULT,'NURETTİN','BELEK')
    RETURNING yazarno;
	
	
	7) 10B sınıfındaki öğrenci numarası 3 olan öğrenciyi 10C sınıfına geçirin.

    UPDATE "kütüphane".ogrenci
    SET sinif = '11B'
    WHERE sinif = '11C' AND ogrno = '3';
	
	
	8) 9A sınıfındaki tüm öğrencileri 10A sınıfına aktarın
	
    UPDATE "kütüphane".ogrenci
    SET sinif = '9C'
    WHERE sinif = '9B';
	
	9) Tüm öğrencilerin puanını 5 puan arttırın.
	
    UPDATE "kütüphane".ogrenci
    SET puan = puan + 5;
	
	10) 25 numaralı yazarı silin.
    
    DELETE FROM "kütüphane".yazar
    WHERE yazarno = 25;

	11) Doğum tarihi null olan öğrencileri listeleyin. (insert sorgusu ile girilen 3 öğrenci listelenecektir)
    
    SELECT * FROM "kütüphane".ogrenci
    WHERE dtarih IS NULL;	

	
	12) Doğum tarihi null olan öğrencileri silin. 

    DELETE * FROM "kütüphane".ogrenci
    WHERE dtarih is NULL;
	
	
	13) Kitap tablosunda adı a ile başlayan kitapların puanlarını 2 artırın.

    UPDATE "kütüphane".kitap
    SET puan = puan + 2
    WHERE ad ILIKE 'd%'
	
	
	14) Kişisel Gelişim isimli bir tür oluşturun.
	
    INSERT INTO "kütüphane".tur
    VALUES(DEFAULT,'Kişisel Gelişim');	

	15) Kitap tablosundaki Başarı Rehberi kitabının türünü bu tür ile değiştirin.
    
    UPDATE "kütüphane".kitap
    SET turno = 7
    WHERE ad = 'DUNE';
	
	
	16) Öğrenci tablosunu kontrol etmek amaçlı tüm öğrencileri görüntüleyen "ogrencilistesi" adında bir prosedür oluşturun.
    
    //Aşağıdaki koddan emin değilim. Tekrar kontrol edilecek. Çünkü CALL "kütüphane".ogrencilistesi() ya da CALL ogrencilistesi() dediğimde çalışmıyor. 

    CREATE OR REPLACE PROCEDURE "kütüphane".ogrencilistesi()
    LANGUAGE plpgsql
    AS $$
    BEGIN
    SELECT *
    FROM "kütüphane".ogrenci;
    END;	 
    $$;


	17) Öğrenci tablosuna yeni öğrenci eklemek için "ekle" adında bir prosedür oluşturun.
	
    CREATE OR REPLACE PROCEDURE ekle(
      IN yeni_ad VARCHAR(50),
      IN yeni_soyad VARCHAR(50),
      IN yeni_numara INTEGER
    )
    LANGUAGE plpgsql
    AS $$
    BEGIN
        INSERT INTO "kütüphane".ogrenci (ad, soyad, numara) VALUES (yeni_ad,
    yeni_soyad, yeni_numara);
        COMMIT;
    END;
    $$;
	
	18) Öğrenci noya göre öğrenci silebilmeyi sağlayan "sil" adında bir prosedür oluşturun.
	
    CREATE OR REPLACE PROCEDURE "kütüphane".ekle(	  
      IN yeni_ad VARCHAR(255),
      IN yeni_soyad VARCHAR(255),
	  IN yeni_dtarih VARCHAR(255),
	  IN yeni_cinsiyet VARCHAR(255),
	  IN yeni_sinif VARCHAR(5),
	  IN yeni_puan INTEGER
      
    )
    LANGUAGE plpgsql
    AS $$
    BEGIN
        INSERT INTO "kütüphane".ogrenci (ad, soyad, dtarih, cinsiyet, sınıf, puan) VALUES (yeni_ad,
    yeni_soyad, yeni_dtarih, yeni_cinsiyet, yeni_sinif, yeni_puan);
        COMMIT;
    END;
    $$;

    CALL "kütüphane".ekle('John','Doe','01-01-1990', 'Erkek', '11C', 78)
	
	19) Öğrenci numarasını kullanarak kolay bir biçimde öğrencinin sınıfını değiştirebileceğimiz bir prosedür oluşturun.

    CREATE OR REPLACE PROCEDURE "kütüphane".sinif_degistir(
    IN yeni_ogrno INTEGER,
    IN yeni_sinif VARCHAR(5)
    )
    LANGUAGE plpgsql
    AS $$
    BEGIN
    UPDATE "kütüphane".ogrenci
    SET sinif = yeni_sinif
    WHERE ogrno = yeni_ogrno;
    COMMIT;
    END;
    $$;

    CALL "kütüphane".sinif_degistir(13,'11D');
	
	
	20) Öğrenci adı ve soyadını "Ad Soyad" olarak birleştirip, ad soyada göre kolayca arama yapmayı sağlayan bir prosedür yazın.
    
    Aşağıdaki prosedürü oluşturup çağırdığımda sadece success mesajı alıyorum. Bana öğrenciyi dönmüyor. Tekrar kontrol edilecek. 	
 
    CREATE OR REPLACE PROCEDURE "kütüphane".ogrenci_ara(
    IN aranan_ad_soyad VARCHAR(255)
    )
    LANGUAGE plpgsql
    AS $$
    BEGIN
    SELECT *
    FROM "kütüphane".ogrenci
    WHERE CONCAT(ad, ' ', soyad) = aranan_ad_soyad;
    END;
    $$;
	
	21) Daha önceden oluşturduğunu tüm prosedürleri silin.
	
	
	#Esnek görevler (Esnek görevlerin hepsini Select in Select ile gerçekleştirmeniz beklenmektedir.)
	22) Select in select yöntemiyle dram türündeki kitapları listeleyiniz.
	
	
	23) Adı e harfi ile başlayan yazarların kitaplarını listeleyin.
	
	
	24) Kitap okumayan öğrencileri listeleyiniz.
	
	
	25) Okunmayan kitapları listeleyiniz

	
	26) Mayıs ayında okunmayan kitapları listeleyiniz.
