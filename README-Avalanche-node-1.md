# KyveNetwork
Kyve Network Avalanche Havuzu 1 Node kurulumu

Node uygulama sitesi https://app.kyve.network/#/ 

# Sistem Gereksinimleri
1 vCPU <br>
4 GB RAM <br>
1 GB DISK <br>
Ubuntu 20.04

# Kurulum --- 1. Adım
2 adet cüzdana ihtiyacımız var Bunlardan birincisi arweave , ikincisi Keplr  <br>
Bu iki cüzdanıda kurulumunu yapınız arwwave cüzdanı kurulum yapıldığı sırada json dosyası verecektir bunu bilgisayarınıza kaydediniz. Daha sonra bu json dosyasının ismini arweave.json olarak değiştiriniz bunu winscp ile kyve klasörünün içine atacağız. <br><br>
Keplr cüzdanımız kurduktan sonra https://app.kyve.network/#/ adresine girerek Korellia ağı otomatik olarak eklenmektedir. Daha sonra site üzerinden Faucet bölümünden test token talep ediniz. 

# Kurulum --- 2. Adım

sudo apt update && apt install unzip<br>
mkdir -p /root/kyve<br>
cd /root/kyve && wget https://github.com/KYVENetwork/evm/releases/download/v1.0.3/evm-linux.zip && unzip evm-linux.zip<br>
chmod +x evm-linux <br>
cd /root/kyve && ./evm-linux --version 

# Kurulum --- 3. Adım

sudo tee /etc/systemd/system/kyved.service > /dev/null <<EOF<br>
[Unit]<br>
Description=Kyve Node<br>
After=network-online.target<br>
[Service]<br>
User=root<br>
WorkingDirectory=/root/kyve/<br>
ExecStart=/root/kyve/evm-linux -m "Keplr cüzdan mnemonic phrase" -k /root/kyve/arweave.json -p 1 -v -s stake miktarınız<br>
Restart=on-failure<br>
RestartSec=3<br>
LimitNOFILE=65535<br>
[Install]<br>
WantedBy=multi-user.target<br>
EOF

# Node Başlatmak 4. Adım
sudo systemctl daemon-reload && sudo systemctl enable kyved && sudo systemctl start kyved  

# Loglara bakmak
journalctl -f -u kyved
