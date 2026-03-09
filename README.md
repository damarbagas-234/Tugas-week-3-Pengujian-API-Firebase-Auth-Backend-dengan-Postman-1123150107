<img width="1890" height="946" alt="Screenshot 2026-03-09 145709" src="https://github.com/user-attachments/assets/a6b4c5a3-aeee-470f-b142-b276516e5ef2" /> Langkah pertama buka aplikasi Firebase.com di google
<img width="1608" height="905" alt="Screenshot 2026-03-09 145750" src="https://github.com/user-attachments/assets/0db3f34e-73de-4a59-8ced-a5252c8d9a70" /> buka console, di sini saya kagak buat proyek firebase baru di karenakan sudah buat sebelumnya
buka Project settings, saya pakai Web apps yang udah lama di buat makanya saya kagak perlu tambahkan aplikasi lagi
Catat WEB API KEY karena akan di butuhkan pada langkah langkah di Postman <img width="1142" height="747" alt="Screenshot 2026-03-09 145923" src="https://github.com/user-attachments/assets/5858f533-b6cd-43f4-82d4-77a77d2504de" /> 
buka POSTMAN
Ubah metode HTTP menjadi POST
Masukkan URL berikut di kolom URL : https://identitytoolkit.googleapis.com/v1/accounts:signUp?key={{FIREBASE_API_KEY}}
Pindah ke tab Headers, tambahkan : key = Content-Type, Value = application/json <img width="1080" height="266" alt="Screenshot 2026-03-09 150010" src="https://github.com/user-attachments/assets/6a02171a-d9e8-492a-9f00-45ea1eada8a1" />
Pindah ke tab Body, pilih raw, lalu pastikan format di ujung kanan adalah JSON. Masukkan ini: { "email": "{{USER_EMAIL}}", "password": "{{USER_PASSWORD}}", "returnSecureToken": true } <img width="1079" height="298" alt="Screenshot 2026-03-09 150042" src="https://github.com/user-attachments/assets/69cb0b8b-508a-4a25-a6a7-1dc919b5cfec" />
Masuk ke bagian Scripts, pastikan pilih yang Post-reponse: const json = pm.response.json(); if (pm.response.code === 200) { pm.environment.set("FIREBASE_ID_TOKEN", json.idToken); pm.environment.set("FIREBASE_LOCAL_ID", json.localId); pm.environment.set("FIREBASE_REFRESH_TOKEN", json.refreshToken); console.log("Register sukses. UID:", json.localId); console.log("PERHATIAN: Email belum diverifikasi. Lanjut ke Step 2."); } else { console.log("Register gagal:", json.error.message); } Ini salah satu tujuannya adlaah untuk memasukkan ke id token yang dihasilkan langsung ke Enviromnent, Sehingga kedepannya engga perlu secara manual memasukkan ID_Token <img width="1073" height="492" alt="Screenshot 2026-03-09 150117" src="https://github.com/user-attachments/assets/eb281256-57ff-46be-9f6c-73cd2adfcd04" />
lalu kita klik send
Masukkan URL berikut di kolom URL : https://identitytoolkit.googleapis.com/v1/accounts:sendOobCode?key={{FIREBASE_API_KEY}}
Pindah ke tab Body, pilih raw, lalu pastikan format di ujung kanan adalah JSON. Masukkan ini: { "requestType": "VERIFY_EMAIL", "idToken": "{{FIREBASE_ID_TOKEN}}" } <img width="1072" height="487" alt="Screenshot 2026-03-09 150220" src="https://github.com/user-attachments/assets/51de5edf-5b42-4c2f-995d-52c095f1f020" />
Masuk ke bagian Scripts, pastikan pilih yang Post-reponse: if (pm.response.code === 200) { const json = pm.response.json(); console.log("Email verifikasi dikirim ke:", json.email); console.log("Sekarang buka inbox email dan klik link verifikasi."); console.log("Setelah klik, lanjut ke Step 3 untuk cek status."); } else { console.log("Gagal kirim email:", pm.response.json().error.message); } <img width="1072" height="488" alt="Screenshot 2026-03-09 150307" src="https://github.com/user-attachments/assets/286fdd89-ffc3-4267-991b-400b6ec93e9a" />
Lalu terakhir Klik Send Yang terjadi di background:
Firebase menerima request ini dan membuat "action link" unik
Firebase mengirim email ke user berisi link: https://muhamaddamarbagas.firebaseapp.com/__/auth/action?mode=verifyEmail
User membuka email dan klik link tersebut
Status emailVerified user di Firebase berubah menjadi true
<img width="1313" height="796" alt="Screenshot 2026-03-09 150419" src="https://github.com/user-attachments/assets/454cf28a-1f07-44bc-982b-3f033e589ab7" />
