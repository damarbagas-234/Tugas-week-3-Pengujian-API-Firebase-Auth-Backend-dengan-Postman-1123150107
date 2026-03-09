<img width="1890" height="946" alt="Screenshot 2026-03-09 145709" src="https://github.com/user-attachments/assets/a6b4c5a3-aeee-470f-b142-b276516e5ef2" /> Langkah pertama buka aplikasi Firebase.com di google
<img width="1890" height="946" alt="Screenshot 2026-03-09 145709" src="https://github.com/user-attachments/assets/586ef98a-4421-4323-8db2-31dbf0af41d9" /> buka console, di sini saya kagak buat proyek firebase baru di karenakan sudah buat sebelumnya
buka Project settings, saya pakai Web apps yang udah lama di buat makanya saya kagak perlu tambahkan aplikasi lagi
Catat WEB API KEY karena akan di butuhkan pada langkah langkah di Postman <img width="1890" height="946" alt="Screenshot 2026-03-09 145709" src="https://github.com/user-attachments/assets/8c10d494-c917-4582-a573-68c79de67f54" />
buka POSTMAN
Ubah metode HTTP menjadi POST
Masukkan URL berikut di kolom URL : https://identitytoolkit.googleapis.com/v1/accounts:signUp?key={{FIREBASE_API_KEY}}
Pindah ke tab Headers, tambahkan : key = Content-Type, Value = application/json <img width="1890" height="946" alt="Screenshot 2026-03-09 145709" src="https://github.com/user-attachments/assets/0ddd0d5c-6776-490a-92fa-6b969fc579ba" />
Pindah ke tab Body, pilih raw, lalu pastikan format di ujung kanan adalah JSON. Masukkan ini: { "email": "{{USER_EMAIL}}", "password": "{{USER_PASSWORD}}", "returnSecureToken": true } <img width="1890" height="946" alt="Screenshot 2026-03-09 145709" src="https://github.com/user-attachments/assets/3f24863d-fc3c-44b1-94b0-75f321074ae1" />
Masuk ke bagian Scripts, pastikan pilih yang Post-reponse: const json = pm.response.json(); if (pm.response.code === 200) { pm.environment.set("FIREBASE_ID_TOKEN", json.idToken); pm.environment.set("FIREBASE_LOCAL_ID", json.localId); pm.environment.set("FIREBASE_REFRESH_TOKEN", json.refreshToken); console.log("Register sukses. UID:", json.localId); console.log("PERHATIAN: Email belum diverifikasi. Lanjut ke Step 2."); } else { console.log("Register gagal:", json.error.message); } Ini salah satu tujuannya adlaah untuk memasukkan ke id token yang dihasilkan langsung ke Enviromnent, Sehingga kedepannya engga perlu secara manual memasukkan ID_Token <img width="1890" height="946" alt="Screenshot 2026-03-09 145709" src="https://github.com/user-attachments/assets/a36ccd34-888a-4b1f-b6d0-6097454ac66f" />
lalu kita klik send
Masukkan URL berikut di kolom URL : https://identitytoolkit.googleapis.com/v1/accounts:sendOobCode?key={{FIREBASE_API_KEY}}
Pindah ke tab Body, pilih raw, lalu pastikan format di ujung kanan adalah JSON. Masukkan ini: { "requestType": "VERIFY_EMAIL", "idToken": "{{FIREBASE_ID_TOKEN}}" } <img width="1890" height="946" alt="Screenshot 2026-03-09 145709" src="https://github.com/user-attachments/assets/f02cc9b5-e93c-42ac-95cb-a6f96bdaa10c" />
Masuk ke bagian Scripts, pastikan pilih yang Post-reponse: if (pm.response.code === 200) { const json = pm.response.json(); console.log("Email verifikasi dikirim ke:", json.email); console.log("Sekarang buka inbox email dan klik link verifikasi."); console.log("Setelah klik, lanjut ke Step 3 untuk cek status."); } else { console.log("Gagal kirim email:", pm.response.json().error.message); } <img width="1890" height="946" alt="Screenshot 2026-03-09 145709" src="https://github.com/user-attachments/assets/aed6e94b-c0f3-4194-a79a-cf6cf58c320b" />
Lalu terakhir Klik Send Yang terjadi di background:
Firebase menerima request ini dan membuat "action link" unik
Firebase mengirim email ke user berisi link: https://muhamaddamarbagas.firebaseapp.com/__/auth/action?mode=verifyEmail
User membuka email dan klik link tersebut
Status emailVerified user di Firebase berubah menjadi true
<img width="1890" height="946" alt="Screenshot 2026-03-09 145709" src="https://github.com/user-attachments/assets/7606592c-fffd-4ca0-8cc4-f83b11e7ebea" />
