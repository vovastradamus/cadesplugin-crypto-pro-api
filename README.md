# cadesplugin-crypto-pro-api

Библиотека предоставляет API для работы c cadesplugin и Крипто Про

## Install

npm i cadesplugin-crypto-pro-api

## API

### about()

Выводит информацию о верисии плагина и так далее

### getCertsList()

Получает массив валидных сертификатов

### currentCadesCert(thumbprint)

Получает сертификат по thumbprint значению сертификата

### getCert(thumbprint)

Получает сертификат по thumbprint значению сертификата.
С помощью этой функции в сертификате доступны методы для парсинга информации о сертификате

### signBase64(thumbprint, base64, type)

Подписать строку в формате base64

### signXml(thumbprint, xml, cadescomXmlSignatureType)

Подписать строку в формате XML

## Custom certs format API

### friendlySubjectInfo()

Возвращает распаршенную информацию о строке subjectInfo

### friendlyIssuerInfo()

Возвращает распаршенную информацию о строке issuerInfo

### friendlyValidPeriod()

Возвращает распаршенную информацию об объекте validPeriod

### possibleInfo(subjectIssuer)

Функция формирует ключи и значения в зависимости от переданного параметра
Доступные параметры 'subjectInfo' и 'issuerInfo'

### friendlyDate(date)

Формирует дату от переданного параметра

### isValid()

Прозиводит проверку на валидность сертификата

## Usage

```js
import ccpa from 'cadesplugin-crypto-pro-api';

/**
 * @async
 * @function doCertsList
 * @description формирует массив сертификатов с оригинальными значениями
 */
async function doCertsList() {
  const certsApi = await ccpa;
  const certsList = await certsApi.getCertsList();

  return certsList;
}

/**
 * @async
 * @function doFriendlyCustomCertsList
 * @description формирует массив сертификатов с кастомными полями
 */
async function doFriendlyCustomCertsList() {
  const certsApi = await ccpa;
  const certsList = await certsApi.getCertsList();

  const friendlyCertsList = certsList.map(cert => {
    const friendlySubjectInfo = cert.friendlySubjectInfo();
    const friendlyIssuerInfo = cert.friendlyIssuerInfo();
    const friendlyValidPeriod = cert.friendlyValidPeriod();
    const {
      to: { ddmmyy, hhmmss }
    } = friendlyValidPeriod;

    return {
      subjectInfo: friendlySubjectInfo,
      issuerInfo: friendlyIssuerInfo,
      validPeriod: friendlyValidPeriod,
      thumbprint: cert.thumbprint,
      title: `${
        friendlySubjectInfo.filter(el => el.value === 'Владелец')[0].text
      }. Сертификат действителен до: ${ddmmyy} ${hhmmss}`
    };
  });
}
```

### License

MIT © [Denis Smorodin](https://github.com/smodean)
