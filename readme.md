# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

## Proje Kurulumu
Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

### Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#### Tablolar 
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]


##### Görevler
Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın. 


MIN-MAX, COUNT-AVG-SUM, GROUP BY, JOINS (INNER, OUTER, LEFT, RIGHT
	#ilk 3 soruyu join kullanmadan yazın.
	1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
	SELECT ograd, ogrsoyad,i.atarih FROM ogrenci AS o, islem as i
	WHERE i.ogrno=o.ogrno

	
	2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

	SELECT t.turadi, k.kitapadi
    FROM tur as t
	INNER JOIN kitap as k on t.turno=k.turno
	WHERE t.turadi IN("Fıkra","Hikaye")
	
	SELECT kitapadi, turadi FROM kitap as k, tur as t
	on k.turno=t.turno
	
	3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.
	
	SELECT o.ogrno, o.ograd,o.ogrsoyad,o.sinif, k.kitapadi FROM ogrenci AS o, islem as i, kitap as k WHERE o.ogrno=i.ogrno AND k.kitapno=i.kitapno AND o.sinif IN ("10A","10B") ORDER BY o.ogrno;


	#join ile yazın
	4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
	
	SELECT o.ograd, o.ogrsoyad, i.atarih FROM ogrenci as o
	LEFT JOIN islem as i ON o.ogrno=i.ogrno

	
	5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
	
	SELECT k.kitapadi, t.turadi FROM kitap as k
	LEFT JOIN tur as t ON k.turno=t.turno 
	WHERE t.turadi IN("Fıkra","Hikaye")

	
	6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.
	
	SELECT o.ogrno, o.ograd, o.ogrsoyad, k.kitapadi FROM ogrenci as o
	LEFT JOIN islem as i ON o.ogrno=i.ogrno
	LEFT JOIN kitap as k ON k.kitapno=i.kitapno
	WHERE o.sinif IN("10A","10B") AND k.kitapadi IS NOT NULL
	ORDER BY o.ograd
	
	7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.
	
	SELECT o.ogrno, o.ograd, i.atarih FROM ogrenci as o
	LEFT JOIN islem as i ON o.ogrno=i.ogrno


	8) Kitap almayan öğrencileri listeleyin.

	SELECT o.ogrno, o.ograd, i.atarih FROM ogrenci as o
	LEFT JOIN islem as i ON o.ogrno=i.ogrno
	WHERE i.atarih IS NULL
	
	9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.
	
	SELECT k.kitapno, k.kitapadi, COUNT(i.kitapno) as "KAC DEFA ALINDI" 
	FROM kitap AS k 
	INNER JOIN islem AS i ON i.kitapno = k.kitapno 
	GROUP BY k.kitapno, k.kitapadi 
	ORDER BY k.kitapno

	10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

	SELECT k.kitapno, k.kitapadi, COUNT(i.kitapno) as "KAC DEFA ALINDI" 
	FROM kitap AS k 
	LEFT JOIN islem AS i ON i.kitapno = k.kitapno 
	GROUP BY k.kitapno, k.kitapadi 
	ORDER BY k.kitapno

	11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.
	
	SELECT o.ograd, o.ogrsoyad, k.kitapadi
	FROM ogrenci as o
	LEFT JOIN islem AS i ON i.ogrno = o.ogrno
	LEFT JOIN kitap AS k ON k.kitapno=i.kitapno 
	GROUP BY o.ograd,o.ogrsoyad,k.kitapadi

	12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.
	
	
	13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.
	
	
	14) Tüm kitapların ortalama sayfa sayısını bulunuz.
	#AVG
	
	SELECT AVG(sayfasayisi) as ortalama_sayfa_sayisi FROM kitap;

	15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.
	
	SELECT kitapadi,sayfasayisi FROM kitap
	WHERE sayfasayisi>(SELECT AVG(sayfasayisi) FROM kitap);
	
	16) Öğrenci tablosundaki öğrenci sayısını gösterin
	
	SELECT COUNT(*) as "TOPLAM_OGRENCI" FROM ogrenci
	
	17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.
	
	SELECT COUNT(*) as "TOPLAM_OGRENCI" FROM ogrenci
	
	18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.
	
	SELECT COUNT(DISTINCT ograd) as "FARKLI_OGRENCI_ISMI" FROM ogrenci
	
	19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	

	
	20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	
	SELECT kitapadi, sayfasayisi from kitap LIMIT 1;
	
	21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	
	SELECT sayfasayisi from kitap ORDER BY sayfasayisi ASC LIMIT 1 ;

	22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	
	SELECT kitapadi, sayfasayisi from kitap ORDER BY sayfasayisi ASC LIMIT 1 ;
	
	23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.
	
	SELECT k.kitapadi, k.sayfasayisi,t.turadi FROM kitap AS k
	LEFT JOIN tur AS t ON t.turno=k.turno
	WHERE t.turadi="Dram"
	ORDER BY k.sayfasayisi DESC LIMIT 1;
	
	24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.
	
	SELECT SUM(k.sayfasayisi) FROM kitap AS k
	RIGHT JOIN islem AS i ON i.kitapno=k.kitapno
	RIGHT JOIN ogrenci AS o ON o.ogrno=i.ogrno
	WHERE o.ogrno=15;
	
	25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

	SELECT ograd, COUNT(ograd) AS "İSİM SAYİSİ" FROM ogrenci AS o
	GROUP BY ograd
	
	26) Her sınıftaki öğrenci sayısını bulunuz.
	
	SELECT sinif, COUNT(*) as X FROM ogrenci
	GROUP BY sinif

	
	27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.
	
	SELECT sinif, cinsiyet, COUNT(*) as X FROM ogrenci
	GROUP BY sinif, cinsiyet
	
	28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.
	
	SELECT o.ograd, o.ogrsoyad, SUM(k.sayfasayisi) AS "TOPLAM_SAYFA" FROM ogrenci AS o
	LEFT JOIN islem as i ON i.ogrno=o.ogrno
	LEFT JOIN kitap as k ON k.kitapno=i.islemno
	GROUP BY o.ograd, o.ogrsoyad
	ORDER BY TOPLAM_SAYFA DESC
	
	29) Her öğrencinin okuduğu kitap sayısını getiriniz.
