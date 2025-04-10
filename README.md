# HGang Minecraft Server
Server ini menggunakan docker compose untuk memudahkan set up.

## Cara Set Up
1. Pastikan server VPS sudah ter-install docker
2. Pastikan sudah set-up TCP shield dan terkonek ke CNAME domain. Cek di dashboardnya TCPShield
3. Pastikan backup sudah masuk ke folder `/data` untuk load world yang ada. Jika belum ada world, bisa jalankan langsung docker composenya.
4. Run `docker compose up -d` untuk menjalankan