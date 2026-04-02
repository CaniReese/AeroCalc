# AeroCalc - Profesyonel İHA Performans Analiz Aracı

AeroCalc, insansız hava araçlarının (İHA/Drone) uçuş performansını, batarya ömrünü ve görev verimliliğini yüksek doğrulukla hesaplayan profesyonel bir web tabanlı analiz aracıdır.

## Temel Özellikler
- **Gerçek Zamanlı Hesaplama:** Girdi değişimlerine anında tepki veren dinamik motor.
- **Atmosferik Modelleme:** İrtifaya bağlı hava yoğunluğu değişimlerini hesaba katar.
- **Görev Analizi:** Tırmanma, seyir ve alçalma fazları için enerji tüketimi tahmini.
- **Aerodinamik Değerlendirme:** Sürükleme kuvveti, eğim açısı ve stall riski analizleri.
- **Excel Entegrasyonu:** Projeleri Excel formatında kaydetme ve yeniden yükleme.

## Hesaplama Metodolojisi ve Formüller

Aşağıda AeroCalc motoru tarafından kullanılan temel fiziksel ve mühendislik formülleri yer almaktadır.

### 1. Batarya Parametreleri
Batarya paketinin toplam kapasitesi ve enerji değerleri şu formüllerle hesaplanır:

- **Nominal Voltaj ($V_{nom}$):**
  $$V_{nom} = V_{hücre} \times S$$
  *(Burada S, seri bağlı hücre sayısıdır)*

- **Toplam Kapasite ($Ah$):**
  $$C_{toplam} = C_{hücre} \times P$$
  *(Burada P, paralel bağlı hücre sayısıdır)*

- **Toplam Enerji ($Wh$):**
  $$E_{toplam} = C_{toplam} \times V_{nom}$$

- **Kullanılabilir Enerji ($Wh$):**
  $$E_{usable} = E_{toplam} \times \frac{\text{Deşarj Derinliği (DoD) \%}}{100}$$

### 2. Atmosfer Modeli
İrtifa arttıkça hava yoğunluğu azalır, bu da motorların aynı itkiyi üretmek için daha fazla güç harcamasına neden olur.

- **Hava Yoğunluğu ($\rho$):**
  $$\rho = \frac{p \cdot M}{R \cdot T}$$
  Burada $p$ basıncı ve $T$ sıcaklığı irtifaya bağlı olarak barometrik formül ile güncellenir.
  
- **Güç Ölçekleme Faktörü:**
  Hover (asılı kalma) gücü, hava yoğunluğunun karekökü ile ters orantılı olarak artar:
  $$P_{irtifa} = P_{SL} \times \sqrt{\frac{\rho_{SL}}{\rho_{irtifa}}}$$

### 3. Hover (Havada Asılı Kalma) Analizi
Aşağıdaki hesaplamalar, kullanıcının girdiği motor tablosundaki verilerin lineer interpolasyonu ile gerçekleştirilir.

- **Motor Başına İtki:**
  $$T_{motor} = \frac{\text{Toplam Ağırlık (MTOW)}}{\text{Motor Sayısı}}$$

- **Toplam Hover Gücü ($W$):**
  $$P_{hover} = (P_{motor} \times \text{Motor Sayısı}) + (P_{avi} + P_{fay}) \times 1.05$$
  *(1.05 katsayısı ESC ve sistem kayıplarını temsil eder)*

- **Uçuş Süresi (Dakika):**
  $$T_{endurance} = \frac{E_{usable}}{P_{hover}} \times 60$$

- **Servis Tavanı (Service Ceiling):**
  Maksimum itkinin, toplam ağırlığa eşit olduğu maksimum irtifa olarak hesaplanır. Hava yoğunluğu azaldıkça motorun üretebileceği maksimum itki doğrusal oranda azalır.

### 4. Görev ve Aerodinamik Analiz
İleri uçuş sırasında İHA, hava direncini (drag) yenmek zorundadır.

- **Aerodinamik Sürükleme Kuvveti ($D$):**
  $$D = \frac{1}{2} \cdot \rho \cdot v^2 \cdot C_d \cdot A$$
  *($v$: hız, $C_d$: sürükleme katsayısı, $A$: ön alan)*

- **Uçuş Eğim Açısı (Tilt Angle - $\theta$):**
  $$ \theta = \arctan\left(\frac{D}{\text{Ağırlık}}\right) $$

- **Gereken Toplam İtki:**
  $$T_{total} = \sqrt{\text{Ağırlık}^2 + D^2}$$

- **Hatve Hızı (Pitch Speed - $V_p$):**
  Motorun ve pervanenin üretebileceği maksimum teorik hız limiti:
  $$V_p = \frac{RPM \times \text{Pitch (inç)} \times 0.0254}{60}$$
  *($RPM$, yük altındaki uçuş devridir, yaklaşık $Kv \times V \times 0.85$ olarak modellenir)*

## Lisans
Bu proje eğitim ve mühendislik amaçlı geliştirilmiştir. Tüm hakları saklıdır.

