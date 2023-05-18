# Heartbleed 💔
Heartbleed vulnerability exploit written in Rust

## What is it
Heartbleed is a buffer over-read vulnerability in outdated versions of OpenSSL, caused by a missing bound check in the heartbeat extension.
It can be exploited by crafting a malicious heartbeat packet, with a specified payload lenght bigger than the actual lenght of the payload, 
resulting in a buffer over-read, exposing potentially sensitive data in memory.

This repo is an exploit written in Rust for this vulnerability.

## How to run
### Build a vulnerable version of OpenSSL
```zsh
wget https://www.openssl.org/source/openssl-1.0.1b.tar.gz
tar -xvf openssl-1.0.1b.tar.gz
cd openssl-1.0.1b
./config
make
```

### Generate a new certificate
```zsh
openssl req -x509 -nodes -days 365 -newkey rsa -keyout cert.pem -out cert.pem
```

### Run a vulnerable server
```zsh
<path to vulnerable OpenSSL>/apps/openssl s_server -cert cert.pem
```

### Run the exploit
```zsh
git clone https://github.com/mrgian/heartbleed.git
cd heartbleed
cargo run -- 127.0.0.1:4433
```

The content of memory should be dumped to data.txt


