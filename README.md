# Integrasi Google Sheets dengan Apps Script dan Streamlit

## 📌 Deskripsi Proyek
Proyek ini bertujuan untuk menghubungkan **Google Sheets** dengan aplikasi berbasis **Streamlit** melalui **Google Apps Script** dan **Python**. Data yang disimpan di Google Sheets dapat diakses, diperbarui, ditambahkan, dan dihapus melalui API yang dibuat menggunakan Google Apps Script. API ini kemudian digunakan oleh aplikasi **Streamlit** yang akan menampilkan data tersebut dalam bentuk dashboard interaktif.

## 🛠️ Teknologi yang Digunakan
- **Google Sheets** sebagai penyimpanan data
- **Google Apps Script** untuk membuat API backend
- **Python (requests, pandas, streamlit)** untuk frontend
- **GitHub** untuk version control dan deployment
- **Streamlit** untuk tampilan dashboard

## 📜 Fitur Utama
1. **Membaca Data** dari Google Sheets dan menampilkannya di Streamlit.
2. **Menambahkan Data** ke Google Sheets melalui API.
3. **Memperbarui Data** yang sudah ada.
4. **Menghapus Data** dari Google Sheets.
5. **Auto-refresh data** pada aplikasi Streamlit setiap ada perlakuan pada data.

## 🚀 Cara Menggunakan
### **1. Setup Google Apps Script**
- Buka **Google Apps Script**
- Buat proyek baru dan salin kode berikut:

```javascript
function doGet(e) {
  var sheet = SpreadsheetApp.openById("YOUR_SPREADSHEET_ID").getSheetByName("data");
  var data = sheet.getDataRange().getValues();
  
  var result = [];
  for (var i = 1; i < data.length; i++) {
    result.push({
      name: data[i][0],
      age: data[i][1],
      city: data[i][2]
    });
  }
  
  return ContentService.createTextOutput(JSON.stringify(result))
    .setMimeType(ContentService.MimeType.JSON);
}

function doPost(e) {
  var sheet = SpreadsheetApp.openById("YOUR_SPREADSHEET_ID").getSheetByName("data");
  var data = JSON.parse(e.postData.contents);

  if (data.action == "add") {
    sheet.appendRow([data.name, data.age, data.city]);
    return ContentService.createTextOutput("Success").setMimeType(ContentService.MimeType.TEXT);
  }

  if (data.action == "update") {
    var values = sheet.getDataRange().getValues();
    for (var i = 1; i < values.length; i++) {
      if (values[i][0] == data.name) {
        sheet.getRange(i + 1, 2).setValue(data.age);
        sheet.getRange(i + 1, 3).setValue(data.city);
        return ContentService.createTextOutput("Updated").setMimeType(ContentService.MimeType.TEXT);
      }
    }
    return ContentService.createTextOutput("Not Found").setMimeType(ContentService.MimeType.TEXT);
  }

  if (data.action == "delete") {
    var values = sheet.getDataRange().getValues();
    for (var i = 1; i < values.length; i++) {
      if (values[i][0] == data.name) {
        sheet.deleteRow(i + 1);
        return ContentService.createTextOutput("Deleted").setMimeType(ContentService.MimeType.TEXT);
      }
    }
    return ContentService.createTextOutput("Not Found").setMimeType(ContentService.MimeType.TEXT);
  }

  return ContentService.createTextOutput("Invalid Action").setMimeType(ContentService.MimeType.TEXT);
}
```

- **Deploy** script sebagai **Web App** dan salin URL-nya.

### **2. Setup Aplikasi Streamlit**
- Buat file `try1.py` 

### **3. Deploy ke GitHub dan Jalankan di Streamlit Cloud**
1. **Push kode ke GitHub**
2. **Deploy ke Streamlit Cloud**

## 📌 Kesimpulan
Dengan proyek ini, Anda dapat menghubungkan Google Sheets ke aplikasi berbasis web dengan mudah menggunakan Google Apps Script dan Streamlit. Anda bisa menambahkan fitur lebih lanjut seperti autentikasi atau analisis data lebih lanjut.

---
💡 **Untuk pertanyaan atau saran, silakan buka issue di repository GitHub Anda!**

