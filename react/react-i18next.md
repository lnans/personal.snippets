# React i18next

## Installation

```bash
yarn add i18next i18next-http-backend i18next-browser-languagedetector react-i18next
```

## Config

This config use translation files under `public/locales/{{ns}}/{{lng}}.json`

```ts
import i18next from "i18next";
import LanguageDetector from "i18next-browser-languagedetector";
import HttpApi from "i18next-http-backend";
import { initReactI18next } from "react-i18next";

const langDetectorOptions = {
  // order and from where user language should be detected
  order: ["cookie", "localStorage", "navigator"],

  // keys or params to lookup language from
  lookupCookie: "locale",
  lookupLocalStorage: "locale",

  // cache user language on
  caches: ["localStorage", "cookie"],
  excludeCacheFor: ["cimode"], // languages to not persist (cookie, localStorage)

  // only detect languages that are in the whitelist
  checkWhitelist: true,

  // Only if all files are en.json, fr.json ect...
  convertDetectedLanguage: (lng: string) => lng.split("-")[0],
};

i18next
  .use(HttpApi)
  .use(initReactI18next)
  .use(LanguageDetector)
  .init({
    fallbackLng: ["fr"],
    supportedLngs: ["fr", "en"],
    detection: langDetectorOptions,
    backend: {
      loadPath: "/locales/{{ns}}/{{lng}}.json",
    },
    ns: [], // Disable fallback NS behavior,
  });

export { i18next };
```

## Usage

```tsx
import { I18nextProvider } from "react-i18next";
import { i18next } from "<exportedConfiguration>";

export function App() {

  return (
    <I18nextProvider i18n={i18next}>
      {/* Your app goes here */}
    </I18nextProvider>;
  )
}
```
