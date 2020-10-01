## erlang 19.0

openssl 1.0.2

```bash
git clone --depth=1 -b OpenSSL_1_0_2-stable https://github.com/openssl/openssl.git
cd openssl
export CFLAGS=-fPIC
./config shared --prefix=~/opt/ssl-1.0.2-stable --openssldir=~/opt/ssl-1.0.2-stable
make
make test
make install
```

kerberos lib

```bash
sudo apt install libkrb5-dev
```

erlang

```bash
asdf plugin-add erlang

export KERL_CONFIGURE_OPTIONS="--with-ssl=/home/aurelien/opt/ssl-1.0.2-stable"
asdf install erlang 19.0
asdf global erlang 19.0
```

## elixir

```
asdf plugin-add elixir
asdf install elixir 1.3.4
asdf global elixir 1.3.4
```
