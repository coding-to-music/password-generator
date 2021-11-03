# Password Generator and Strength Checker

## Deployed at... [passwrd.pages.dev](https://passwrd.pages.dev)

Password-generator is a basic React web application for generating secure, random passwords with the option of customising the length, the appearance of numbers, uppercase and lowercase letters and symbols.
The strength of the password is also checked and is shown in a range of colors from 0 - no color at all, through to 5 - green.
At the same time, the password is checked with [HaveIBeenPwned](https://haveibeenpwned.com/Passwords) to see if it is contained in any data breaches.
Because I'm obsessed with the weather and how I am seen online, a Cloudflare worker returns the users IP, City, Country and uses that information to lookup the current weather.

## Get Started

If you need to install yarn
```java
npm install -g yarn
```

### React

```java
cd src
yarn start
```

> Development server on localhost:3000

### Cloudflare workers

```java
cd wrangler
```

wrangler dev
```java
wrangler dev
```

Output
```java
wrangler dev
👀  ./index.js 66:52
Module parse failed: Unexpected token (66:52)
You may need an appropriate loader to handle this file type, currently no loaders are configured to process this file. See https://webpack.js.org/concepts#loaders
| // MAIN FUCTION
| async function handleRequest(request) {
>   const openweather = await fetchWeather(request.cf?.city, request.cf?.country, request.cf?.latitude, request.cf?.longitude)
|   const data =
|     request.cf !== undefined ? {
Error: webpack returned an error. You may be able to resolve this issue by running npm install.
```

wrangler secret put API_KEY  //Add secret
```java
wrangler secret put OPENWEATHER_KEY
```

Output
```java
Enter the secret text you'd like assigned to the variable OPENWEATHER_KEY on the script named password-generator:
secretinfohere2ff9acfaf06d6
🌀  Creating the secret for script name password-generator
🌀  Worker password-generator doesn't exist in the API yet. Creating a draft Worker so we can create new secret.
Error: No such file or directory (os error 2)
```

```java
wrangler publish
```

Output
```java
👀  ./index.js 66:52
Module parse failed: Unexpected token (66:52)
You may need an appropriate loader to handle this file type, currently no loaders are configured to process this file. See https://webpack.js.org/concepts#loaders
| // MAIN FUCTION
| async function handleRequest(request) {
>   const openweather = await fetchWeather(request.cf?.city, request.cf?.country, request.cf?.latitude, request.cf?.longitude)
|   const data =
|     request.cf !== undefined ? {
Error: webpack returned an error. You may be able to resolve this issue by running npm install.
```


> see wrangler.toml

### TailwindCSS

`tailwind.config.js` contains classes that are generated dynamically in application

### HaveIBeenPwned

Password breach is available on free API tier at [HaveIBeenPwned](https://haveibeenpwned.com/API/v3)

> In order to protect the value of the source password being searched for, Pwned Passwords also implements a k-Anonymity model that allows a password to be searched for by partial hash. This allows the first 5 characters of a SHA-1 password hash (not case-sensitive) to be passed to the API

### OpenWeather

[Openweather API](https://openweathermap.org/api) has 1,000,000 free monthly requests.
We use the users latitude and longitude to check the weather.
If the user is located in one of

- United States
- Bahamas
- Cayman Islands
- Liberia
- Palau
- The Federated States of Micronesia
- Marshall Islands

We request the data in imperial (Fahrenheit) otherwise we request it in metric (Celcius).

### ZXCVBN

> zxcvbn is a password strength estimator inspired by password crackers. Through pattern matching and conservative estimation, it recognizes and weighs 30k common passwords, common names and surnames according to US census data, popular English words from Wikipedia and US television and movies, and other common patterns like dates, repeats (aaa), sequences (abcd), keyboard patterns (qwertyuiop), and l33t speak.
> [ZXCVBN Github Repo](https://github.com/dropbox/zxcvbn)

### Cloudflare Pages

This is currently deployed on [Pages](https://pages.cloudflare.com/)

### Cloudflare Workers

The serverless function running IP data and getting Weather data is on [Workers](https://workers.cloudflare.com/)

```
cd wrangler
cp wrangler.toml.example wrangler.toml
```

Add `account_id`

> #### account_id required
>
> This is the ID of the account associated with your zone. You might have more than one account, so make sure to use the ID of the account associated with the zone_id you provide, if you provide one. It can also be specified through the CF_ACCOUNT_ID environment variable.

It is safe to commit `account_id` and `zone_id` as per this [Github issue](https://github.com/cloudflare/wrangler/issues/209#issuecomment-541654484)
