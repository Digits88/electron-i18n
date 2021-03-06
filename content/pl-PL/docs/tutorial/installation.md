# Instalacja

> Porady odnośnie instalowania Electrona

W celu zainstalowania prekompilowanych plików Electrona, użyj [`npm`](https://docs.npmjs.com/). Preferowaną metodą instalacji Electrona jest zainstalowanie go jako zależność deweloperską twojej aplikacji:

```sh
npm install electron --save-dev
```

Zobacz [dokumentację systemu kontroli wersji](electron-versioning.md), aby dowiedzieć się więcej o zarządzaniu wersjami Electrona w twoich aplikacjach.

## Instalacja globalna

Możesz także zainstalować komendę `electron` globalnie w twojej zmiennej `$PATH`:

```sh
npm install electron -g
```

## Dostosowywanie

Jeśli chcesz zmienić architekturę pobieranych pakietów (np. `ia32` na maszynie `x64`), możesz użyć flagi `--arch` w komendzie npm, albo ustawić zmienną środowiskową `npm_config_arch`:

```shell
npm install --arch=ia32 electron
```

Dodatkowo, możesz także określić platformę (np. `win32`, `linux`, itd.), używając flagi `--platform`:

```shell
npm install --platform=win32 electron
```

## Proxy

Jeżeli używasz serwera HTTP Proxy, możesz [użyć tych zmiennych środowiskowych](https://github.com/request/request/tree/f0c4ec061141051988d1216c24936ad2e7d5c45d#controlling-proxy-behaviour-using-environment-variables).

## Rozwiązywanie problemów

Uruchomiając komendę `npm install electron`, niewielka część użytkowników czasami napotyka się na błędy instalacji.

W większości przypadków, błędy te są wynikiem problemów z połączeniem internetowym, a nie błędami pakietu `electron`. Błędy typu `ELIFECYCLE`, `EAI_AGAIN`, `ECONNRESET`, and `ETIMEDOUT` są objawami tego typu błędów z połączeniem. Najlepszym ich rozwiązaniem jest podjęcie próby zmiany sieci, bądź odczekanie chwili i ponowna próba instalacji.

Możesz także spróbować pobrać Electrona bezpośrednio z [electron/electron/releases](https://github.com/electron/electron/releases), jeśli instalacja poprzez `npm` zawodzi.

Jeśli instalacja powoduje błąd `EACCESS`, być może musisz naprawić [uprawnienia npm](https://docs.npmjs.com/getting-started/fixing-npm-permissions).

Jeżeli powyższe błędy nie ustępują, flaga [unsafe-perm](https://docs.npmjs.com/misc/config#unsafe-perm), może wymagać ustawienia wartości "true":

```sh
sudo npm install electron --unsafe-perm=true
```

W przypadku wolnego połączenia z internetem, zalecane jest użycie flagi `--verbose`, celem podglądu postępu pobierania:

```sh
npm install --verbose electron
```

W przypadku, gdy musisz wymusić ponowne pobranie sumy kontrolnej oraz danych, ustaw zmienną środowiskową `force_no_cache` na wartość `true`.