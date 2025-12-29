# Pocket-Teacher: Knowledge Distillation ile Küçük Dil Modeli Eğitimi

Bu proje, büyük dil modellerinin (LLM) bilgi ve muhakeme yeteneklerini,
daha küçük ve verimli modellere aktarmayı amaçlayan
**Pocket-Teacher** adlı bir **Knowledge Distillation (KD)** yaklaşımını sunmaktadır.

Çalışma, tek bir Jupyter Notebook içerisinde,
veri hazırlığından model kurulumuna, eğitimden değerlendirmeye kadar
tüm süreci uçtan uca ve tekrarlanabilir biçimde ele almaktadır.

---

## Projenin Amacı

Güncel büyük dil modelleri yüksek doğruluk ve muhakeme başarısı sunsa da,
yüksek hesaplama maliyetleri nedeniyle pratik uygulamalarda sınırlıdır.

Bu projenin temel amacı şudur:

> Büyük bir öğretmen dil modelinin muhakeme bilgisini,
> minimum performans kaybı ile daha küçük bir öğrenci modele aktarmak.

Bu amaç doğrultusunda,
öğretmen–öğrenci tabanlı Knowledge Distillation yöntemi uygulanmıştır.

---

## Knowledge Distillation Pipeline’ı

<p align="center">
  <img src="figures/pocket_teacher_pipeline.png" width="900"/>
</p>

**Şekil:** Pocket-Teacher Knowledge Distillation eğitim pipeline’ı.

---

## Kullanılan Yöntem

### Veri Hazırlığı

- Muhakeme odaklı veri setleri (ör. GSM8K, MMLU) kullanılmıştır.
- Promptlar talimat (instruction) formatında yeniden yapılandırılmıştır.
- Tokenizasyon işlemi öğrenci modelin tokenizer’ı ile gerçekleştirilmiştir.
- Eğitim verileri batch yapısında hazırlanmıştır.

---

### Model Kurulumu

- **Öğretmen Model**
  - Büyük ölçekli, önceden eğitilmiş bir dil modeli
  - 8-bit / FP16 hassasiyet
  - Parametreler dondurulmuştur (gradient hesaplanmaz)

- **Öğrenci Model**
  - Daha küçük ve hafif bir dil modeli
  - FP16 / bfloat16 hassasiyet
  - Tamamen eğitilebilir

Tüm deneyler **NVIDIA A100 GPU** ortamında yürütülmüştür.

---

### Knowledge Distillation Eğitimi

Eğitim sırasında:

- Öğretmen modelden **yumuşak etiketler (logits)** elde edilmiştir.
- Öğrenci modelin çıktıları ile karşılaştırılmıştır.
- Sert (ground truth) ve yumuşak (teacher) kayıplar birlikte optimize edilmiştir.

Toplam kayıp fonksiyonu:

\[
\mathcal{L} = \alpha \cdot \mathcal{L}_{\text{CE}} + (1 - \alpha) \cdot T^2 \cdot \mathcal{L}_{\text{KL}}
\]

Bu yapı, öğrenci modelin hem doğru cevabı öğrenmesini
hem de öğretmen modelin karar dağılımını taklit etmesini sağlar.

---

### Değerlendirme

- Eğitilmiş öğrenci model, öğretmen model ile aynı test verisi üzerinde değerlendirilmiştir.
- Performans karşılaştırmaları doğruluk ve muhakeme başarısı üzerinden yapılmıştır.
- Sonuçlar, öğrenci modelin önemli ölçüde daha düşük hesaplama maliyetiyle
öğretmene yakın performans sunduğunu göstermektedir.

---

## Akademik Katkılar

Bu çalışmanın başlıca akademik katkıları şunlardır:

1. **Tek notebook içinde uçtan uca Knowledge Distillation uygulaması**  
   Veri hazırlığı, model kurulumu, eğitim ve değerlendirme aşamaları
   bütüncül ve tekrarlanabilir şekilde sunulmuştur.

2. **Muhakeme odaklı veri setleri üzerinde distillation analizi**  
   Öğrenci modelin muhakeme kabiliyetinin korunabilirliği incelenmiştir.

3. **Hibrit kayıp fonksiyonu kullanımı**  
   Sert ve yumuşak etiketlerin birlikte optimize edilmesiyle
   daha dengeli bir öğrenme sağlanmıştır.

4. **Dağıtılabilir küçük model hedefi**  
   Eğitilen öğrenci model, düşük gecikme ve düşük bellek tüketimi
   gerektiren senaryolar için uygun hale getirilmiştir.

---

## Proje Yapısı

Bu proje tek bir Jupyter Notebook üzerinden yürütülmektedir:

.
├── Pocket_Teacher.ipynb # Tüm veri hazırlığı, eğitim ve değerlendirme adımları
├── README.md


Notebook, deneylerin yeniden üretilebilirliğini sağlayacak şekilde
adım adım yapılandırılmıştır.

---

## Lisans ve Kullanım

Bu proje akademik ve eğitim amaçlıdır.
Kullanılan model ve veri setlerinin lisans koşulları ayrıca incelenmelidir.
