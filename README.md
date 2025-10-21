# Kullanım Adımları (n8n Workflow)

## 0) Gemini API anahtarını alın (zorunlu)
- **Gemini API key** için şu adrese gidin: https://aistudio.google.com/api-keys
- Yeni bir **API Key** oluşturun.
- n8n içinde **Credentials** > **New** > **Google Gemini (PaLM) API** seçin.
- Oluşturduğunuz anahtarı bu credential’a yapıştırın ve kaydedin.
- Workflow’taki **Analyze document** düğümü bu credential’ı kullanacak şekilde ayarlı (model: `models/gemini-2.5-flash`).

> Not: Google Cloud üzerinden Vertex AI kullanıyorsanız, aynı düğüm için proje/konum tabanlı yapılandırmayı tercih edebilirsiniz. Bu proje, doğrudan **Gemini API key** ile çalışacak şekilde kurgulanmıştır.


Aşağıdaki adımlar, workflow’u import ettikten sonra çalıştırmadan önce yapmanız gereken zorunlu ayarları içerir.

## 1) Google Drive klasör ID’sini ayarlayın
- **Search files and folders** düğümündeki `queryString` içinde tek tırnakla belirtilen **kaynak klasör ID**’sini kendi klasörünüzün ID’siyle değiştirin.
- Klasör ID’si, Drive URL’sindeki şu kısımdır:  
  `https://drive.google.com/drive/folders/{ID}`
- Örnek sorgu kalıbı: `'YOUR_FOLDER_ID' in parents and mimeType = 'application/pdf' and trashed = false`

## 2) Google Cloud üzerinde yetkilendirme
- `https://console.cloud.google.com/` üzerinden **Drive** ve **Sheets** API’lerini etkinleştirin.
- Bir **Service Account** oluşturun (veya uygun bir API anahtarı edinin).
- n8n içindeki Google bağlantılarınızı bu Service Account ile yapılandırın.
- **Önemli:** Kaynak Drive klasörünü ve hedef Google Sheet’i **Service Account e-postasıyla “Editor (Düzenleyen)”** olarak paylaşın.

## 3) Google Sheets yapılandırması
- **Append or update row in sheet** düğümünde **Spreadsheet ID** ve **Sheet (tablo) adı** alanlarını kendi belgenize göre değiştirin.
- Sütun eşlemesini (ID, KAYNAK, MAKALE ADI, YAZARLAR, AY/YIL, TİP, TİP ADI, ANAHTAR KELİMELER, ARAŞTIRMA SORUSU, YÖNTEM, BULGU VE SONUÇ) kontrol edin.

## 4) (İsteğe bağlı) Taşıma klasörünü ayarlayın
- **Move file** düğümündeki hedef klasör ID’sini (işlenen dosyaların taşınacağı klasör) kendi Drive yapınıza göre güncelleyin.

## 5) Çalıştırma
- n8n editor ekranında **Execute Workflow** tuşuna basın.
- İlk çalıştırmada birkaç PDF ile test edin; Google Sheet’e satırların eklendiğini ve dosyaların doğru klasöre taşındığını doğrulayın.

## 6) Güvenlik
- Service Account anahtar dosyalarını hiçbir zaman repoya koymayın.
- Drive/Sheets erişim izinlerini en az yetki prensibiyle verin.
