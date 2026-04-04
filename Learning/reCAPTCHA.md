**What is reCAPTCHA?**
- [A CAPTCHA](https://datadome.co/guides/captcha/what-is-captcha-and-how-does-it-work/) (acronym for “Completely Automated Public Turing Test to Tell Computers and Humans Apart”) is a security measure designed to differentiate bots from humans, typically with an image or audio challenge.
- ReCAPTCHA is what Google calls [their CAPTCHA system](https://developers.google.com/recaptcha). It was released in 2007 and is currently used by more than 13 million live websites.

**How reCAPTHCA works?**

![[Pasted image 20240423090624.png]]

**Feature Comparison**

![[Pasted image 20240423073250.png]]

**reCAPTCHA Enterprise Pricing**

![[Pasted image 20240423073536.png]]
![[Pasted image 20240423073558.png]]
** Masing2 API Call terhitung 1 Call

![[Pasted image 20240423110418.png]]
![[Pasted image 20240423110454.png]]

![[Pasted image 20240426072154.png]]
![[Pasted image 20240426082243.png]]
Fix 1 juta assessments per bulan gratis

# Langkah Setting Google reCAPTCHA Enterprise

**Setting Google Cloud Console**
1. Sign Up ke Google Cloud Console - https://console.cloud.google.com/security/recaptcha
2. Buat Project Baru / Pilih project yang sudah ada (yang sudah ada OAuth)
3. Pergi ke halaman Google reCAPTCHA Enterprise - https://console.cloud.google.com/apis/library/recaptchaenterprise.googleapis.com
4. Pilih project yang telah dibuat pada langkah 2
5. klik **Enable**

**Buat reCAPTCHA key**
1. Pergi ke halaman Google reCAPTCHA Enterprise - https://console.cloud.google.com/security/recaptcha
2. Pilih projek tang telah dibuat
3. klik **+ CREATE KEY**
4. masukkan display name dari key
5. pilih platform website
6. masukkan domain (localhost jika menjalankan di local)
7. Pilih jenis key yang diinginkan (pilih jenis score)

**Buat Google API key**
1. Buka halaman [Credentials – APIs & Services – sig-sobat-bangun-lo… – Google Cloud console](https://console.cloud.google.com/apis/credentials?project=${NAMA_PROJEK})
2. Buat API Key baru (jika belum ada)

**ENV**
1. Root .env
	1. NEXT_PUBLIC_RECAPTCHA_ENTERPRISE_KEY= (disesuaikan dengan **buat reCAPTCHA key** no 2)
	2. NEXT_PUBLIC_RECAPTCHA_PROJECT_ID= (disesuaikan dengan **Setting Google Cloud Console** no 2)
2. Customer .env
	1. GOOGLE_API_KEY= (disesuaikan dengan **Buat Google API akey** no 2)