# İnşaa Talimatları (Linux)

Electron'u linux üzerinde inşaa etmek için aşağıdaki yönlendirmeleri takip edin.

## Ön gereklilikler

* En az 25GB disk alanı ve 8GB hafıza.
* Python 2.7.x. CentOS gibi bazı dağıtımlar hala Python 2.6.x kullanmakta, dolayısıyla Python versiyonunuzu `python -V` komutu ile ile kontrol edin.
* Node.js. Node'u kurmanın birden fazla yolu var. [nodejs.org](http://nodejs.org)'tan indirip derleyebilirsiniz. Bu şekilde Node'u kullanıcı dizinine standart bir şekilde kurabilirsiniz. Ya da [NodeSource](https://nodesource.com/blog/nodejs-v012-iojs-and-the-nodesource-linux-repositories) gibi depoları deneyebilirsiniz.
* [clang](https://clang.llvm.org/get_started.html) 3.4 veya sonrası.
* GTK+ ve libnotify için geliştirme başlıkları.

Ubuntu üzerinde, aşağıdaki kütüphaneleri kurun:

```sh
$ sudo apt-get install build-essential clang libdbus-1-dev libgtk2.0-dev \
                       libnotify-dev libgnome-keyring-dev libgconf2-dev \
                       libasound2-dev libcap-dev libcups2-dev libxtst-dev \
                       libxss1 libnss3-dev gcc-multilib g++-multilib curl \
                       gperf bison
```

RHEL / CentOS üzerinde aşağıdaki kütüphaneleri kurun:

```sh
$ sudo yum install clang dbus-devel gtk2-devel libnotify-devel \
                   libgnome-keyring-devel xorg-x11-server-utils libcap-devel \
                   cups-devel libXtst-devel alsa-lib-devel libXrandr-devel \
                   GConf2-devel nss-devel
```

Fedora üzerinde aşağıdaki kütüphaneleri kurun:

```sh
$ sudo dnf install clang dbus-devel gtk2-devel libnotify-devel \
                   libgnome-keyring-devel xorg-x11-server-utils libcap-devel \
                   cups-devel libXtst-devel alsa-lib-devel libXrandr-devel \
                   GConf2-devel nss-devel
```

Diğer linux dağıtımları pacman gibi paket yöneticileri üzerinden benzer paketler sunuyor olabilir. Ya da kaynak kodtan derleyebilirsiniz.

## Kodu almak

```sh
$ git clone https://github.com/electron/electron
```

## İlk işleri halletmek

Ilk işleri halleden bootstrap betiği inşaa için gerekli olan bağımlılıkları indirir ve gerekli inşaa dosyalarını hazırlar. Bu betiğin çalışması içın Python 2.7.x'e sahip olmanız gerekir. Bağımlılıkları indirmek bir miktar zaman alabilir. Dikkat ederseniz, Electron'u inşaa etmek için `Makefile` yerine `ninja` kullanıyoruz.

```sh
$ cd electron
$ ./script/bootstrap.py --verbose
```

### Başka sistemler için derleme

Eğer `arm` üstüne inşaa etmek istiyorsanız aşağıdaki bağımlılıkları da indirmeniz gerekir:

```sh
$ sudo apt-get install libc6-dev-armhf-cross linux-libc-dev-armhf-cross \
                       g++-arm-linux-gnueabihf
```

Benzer olarak `arm64` için:

```sh
$ sudo apt-get install libc6-dev-arm64-cross linux-libc-dev-arm64-cross \
                       g++-aarch64-linux-gnu
```

`arm<code> veya <code>ia32` hedefleri için derlerken, `bootstrap.py` betiğine <0>--target_arch</code> parametresi geçmelisiniz:

```sh
$ ./script/bootstrap.py -v --target_arch=arm
```

## İnşaa

Eğer hem `Dağıtım` hem `Hata ayıklama` hedeflerinde inşaa etmek isterseniz:

```sh
$ ./script/build.py
```

Bu betik `out/R` içerisinde oldukça büyük bir Electron çalıştırılabilir dosyası oluşturacaktır. Dosya boyu 1.3 gigabyte'ı geçebilir. Bunun sebebi, Dağıtım hedefli inşaa'nın hata ayıklama sembollerini içeriyor oluşudur. Dosya boyutunu düşürmek içın `create-dist.py` dosyasını çalıştırabilirsiniz:

```sh
$ ./script/create-dist.py
```

Bu betik çalışır bir dağıtımı çok daha ufak boyutlarda `dist`dizinine çıkarır. `create-dist.py` betiğini çalıştırdıktan sonra, hala `out/R` dizini içerisinde bulunan 1.3+ gigabyte'lık dosyayı silmek isteyebilirsiniz.

Ay~i zamanda sadece `Hata ayıklama` hedefleyerek inşaa edebilirsiniz:

```sh
$ ./script/build.py -c D
```

İnşaa tamamlandıktan sonra, `electron` hata ayıklama ikilisini `out/D` dizini altında bulabilirsiniz.

## Temizlik

İnşaa dosyalarını temizlemek için:

```sh
$ npm run clean
```

Sadece `out` ve `dist` dizinlerini temizlemek için:

```sh
$ npm run clean-build
```

**Not:** Her iki clean komutu `bootstrap` betiğinin inşaa öncesi çalıştırılmasını şart koşar.

## Arıza giderme

### Hata mesajı: Error While Loading Shared Libraries: libtinfo.so.5

Önceden inşaa edilmiş `clang` `libtinfo.so.5` bağlantısını yapmaya çalışacak. Makinanın mimarisine göre, uygun `libncurses`'e smylink yapın:

```sh
$ sudo ln -s /usr/lib/libncurses.so.5 /usr/lib/libtinfo.so.5
```

## Testler

[İnşaa Sistemi Genel Görünümü: Testler](build-system-overview.md#tests) sayfasını ziyaret edin

## İleri düzey başlıklar

Varsayılan inşaa konfigurasyon'u belli başlı Linux masaüstü dağıtımları içindir. Özel bir dağıtım veya cihaz için, aşağıdaki bilgiler işinize yarayabilir.

### `libchromiumcontent`'i yerelinize inşaa etme

Önceden inşaa edilmiş`libchromiumcontent`'i kullanmak istemezseniz, `libchromiumcontent`'i kendiniz aşağıdaki adımlarla inşaa edebilirsiniz:

1. [depot_tools](https://chromium.googlesource.com/chromium/src/+/master/docs/linux_build_instructions.md#Install)'u kurun
2. [Ek inşaa bağımlılıklarını](https://chromium.googlesource.com/chromium/src/+/master/docs/linux_build_instructions.md#Install-additional-build-dependencies) kurun
3. Git alt modullerini çekin:

```sh
$ git submodule update --init --recursive
```

1. `--build_release_libcc` argümanını `bootstrap.py` betiğine geçin:

```sh
$ ./script/bootstrap.py -v --build_release_libcc
```

`shared_library` konfigurasyonu varsayılan durumda ekli değildir, yani bu modu kullanarak Electron'un sadece `Dağıtım` versiyonunu inşaa edebilirsiniz:

```sh
$ ./script/build.py -c R
```

### İndirdiğıniz `clang` yerine sistem `clang`'ini kullanmak

Electron, varsayılan olarak Chromium projesi tarafından sağlanan [`clang`](https://clang.llvm.org/get_started.html) ile inşaa edilir. If for some reason you want to build with the `clang` installed in your system, you can call `bootstrap.py` with `--clang_dir=<path>` switch. By passing it the build script will assume the `clang` binaries reside in `<path>/bin/`.

For example if you installed `clang` under `/user/local/bin/clang`:

```sh
$ ./script/bootstrap.py -v --build_release_libcc --clang_dir /usr/local
$ ./script/build.py -c R
```

### Using compilers other than `clang`

To build Electron with compilers like `g++`, you first need to disable `clang` with `--disable_clang` switch first, and then set `CC` and `CXX` environment variables to the ones you want.

For example building with GCC toolchain:

```sh
$ env CC=gcc CXX=g++ ./script/bootstrap.py -v --build_release_libcc --disable_clang
$ ./script/build.py -c R
```

### Ortam Değişkenleri

`CC` ve `CXX` dışında, aşağıdaki ortam değişkenlerini de özel inşaa konfigurasyonları için ayarlayabilirsiniz:

* `CPPFLAGS`
* `CPPFLAGS_host`
* `CFLAGS`
* `CFLAGS_host`
* `CXXFLAGS`
* `CXXFLAGS_host`
* `AR`
* `AR_host`
* `CC`
* `CC_host`
* `CXX`
* `CXX_host`
* `LDFLAGS`

İlgili ortam değişkenleri `bootstrap.py` betiğini çalıştırırken ayarlanmalıdır, `build.py` betiğinin içerisinde çalışmayacaktır.