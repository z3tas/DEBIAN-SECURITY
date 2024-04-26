 Hi all. I have an idea to make for Debian (I'm using LMDE) privacy and security enhancements. I had file with all steps included mac spoofing, mullvad dns, librewolf etc. Anybody want to join?

 My Steps (after fresh install):

 -------------------FIREWALL

sudo ufw enable

-------------------MULLVAD DNS

sudo apt-get update

sudo apt-get install systemd-resolved -Y

sudo systemctl enable systemd-resolved

Open the Settings app and go to Network. Click on the settings icon for your connected network. On the IPv4 and IPv6 tabs, turn off Automatic next to DNS, and leave the DNS field blank, then click on Apply.  Disable and enable the network using the on/off button to make sure it takes effect.

sudo nano /etc/systemd/resolved.conf

#DNS=194.242.2.2#dns.mullvad.net
#DNS=194.242.2.3#adblock.dns.mullvad.net
#DNS=194.242.2.4#base.dns.mullvad.net
#DNS=194.242.2.5#extended.dns.mullvad.net
DNS=194.242.2.6#family.dns.mullvad.net
#DNS=194.242.2.9#all.dns.mullvad.net
DNSSEC=no
DNSOverTLS=yes
Domains=~.

Save the file by pressing Ctrl + O and then Enter, and then Ctrl +X on your keyboard.

sudo ln -sf /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf

sudo systemctl restart systemd-resolved

sudo systemctl restart NetworkManager

resolvectl status

-------------------hBLOCK

https://hblock.molinero.dev/

me@pc:~/_/hblock-3.4.4$ sudo ./hblock

-------------------MAC CHANGER

sudo apt install macchanger

ip a

sudo macchanger -s wlp58s0

sudo macchanger -s enp0s31f6

disconect from internet

sudo macchanger -r wlp58s0

sudo macchanger -r enp0s31f6

-------------------MULLVAD BROWSER

wget --content-disposition https://mullvad.net/en/download/browser/linux-x86_64/latest -P ~/_

cd ~/_

tar -xvf mullvad-browser-linux-x86_64-X.X.tar.xz

cd ~/_/mullvad-browser

./start-mullvad-browser.desktop

cp ~/_/mullvad-browser/start-mullvad-browser.desktop ~/.local/share/applications/

-------------------LIBREWOLF

sudo apt update && sudo apt install -y wget gnupg lsb-release apt-transport-https ca-certificates

distro=$(if echo " una bookworm vanessa focal jammy bullseye vera uma " | grep -q " $(lsb_release -sc) "; then lsb_release -sc; else echo focal; fi)

wget -O- https://deb.librewolf.net/keyring.gpg | sudo gpg --dearmor -o /usr/share/keyrings/librewolf.gpg

sudo tee /etc/apt/sources.list.d/librewolf.sources << EOF > /dev/null
Types: deb
URIs: https://deb.librewolf.net
Suites: $distro
Components: main
Architectures: amd64
Signed-By: /usr/share/keyrings/librewolf.gpg
EOF

sudo apt update

sudo apt install librewolf -y

-------------------BRAVE

sudo apt install curl

sudo curl -fsSLo /usr/share/keyrings/brave-browser-archive-keyring.gpg https://brave-browser-apt-release.s3.brave.com/brave-browser-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/brave-browser-archive-keyring.gpg] https://brave-browser-apt-release.s3.brave.com/ stable main"|sudo tee /etc/apt/sources.list.d/brave-browser-release.list

sudo apt update

sudo apt install brave-browser -y

-------------------KEYBOARD-LAYOUTS

sudo apt-get update

dpkg --list

-------------------UNINSTALL
sudo apt-get install vlc qbittorrent inkscape gimp

sudo apt-get purge celluloid transmission-gtk transmission-common

sudo apt clean
