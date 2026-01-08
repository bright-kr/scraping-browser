# 🚀 Bright Data의 Scraping Browser

*Puppeteer, Selenium, Playwright로 동적 Webスクレイピング을 위한 완전 자동화 헤드리스 브라우저 솔루션입니다. Scraping Browser는 Bright Data의 인프라에서 GUI로 열립니다.*  

[![Promo](https://github.com/luminati-io/LinkedIn-Scraper/raw/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://brightdata.co.kr/products/scraping-browser) 

🔗 **[무료로 시작하기](https://brightdata.co.kr/products/scraping-browser)** | 📖 **[공식 문서](https://docs.brightdata.com/scraping-automation/scraping-browser/introduction)**  

---

## 🔹 개요  
Scraping Browser는 **브라우저 기반 スクレイピング 솔루션**을 완전 호스팅 형태로 제공하며, **내장 プロキシ 관리**, **CAPTCHA 해결**, **고급 웹사이트 언블로킹**을 통해 **다단계 데이터 수집**을 자동화합니다. **Puppeteer, Playwright, Selenium**을 지원하여 **무제한 규모**로 손쉽게 웹 자동화를 수행할 수 있습니다.  

## ✅ Scraping Browser를 사용해야 하는 이유는 무엇입니까?  
- **인프라 오버헤드 없음** – 브라우저 인프라를 유지 관리하지 않고도 API를 통해 브라우저 セッション을 실행 및 확장합니다.  
- **내장 언락 기능** – 내부적으로 **CAPTCHA, ブラウザフィンガープリント, リトライ, JS 렌더링**을 자동 처리합니다.  
- **다단계 내비게이션** – 클릭, 스크롤, 폼 제출, 호버 상호작용을 자동화합니다.  
- **무제한 스케일링** – **레ート制限 없이 수천 개의 同時接続 브라우저 セッション**을 실행할 수 있습니다.  
- **글로벌 지리 액세스** – [**195개 국가에 걸친 72M+ レジデンシャルプロキシ IP**](https://brightdata.co.kr/proxy-types/residential-proxies)로 로컬라이즈된 콘텐츠를 언락합니다.  
- **원활한 디버깅** – **Chrome DevTools 통합**으로 セッション을 실시간으로 모니터링합니다.  

---

## 🚀 시작하기  

### 종속성 설치  
선호하는 웹 자동화 라이브러리와 함께 **Node.js**, **Python**, 또는 **C#**이 설치되어 있는지 확인합니다:  

```sh
# Install Puppeteer
npm install puppeteer-core

# Install Playwright
npm install playwright

# Install Selenium
pip install selenium
```

## 🔧 사용 예시

### Puppeteer 예시 (JavaScript)

```js
const puppeteer = require('puppeteer-core');

const SBR_WS_ENDPOINT = 'wss://brd-customer-<YOUR-USERNAME>-zone-<YOUR-ZONE-NAME>:<YOUR-PASSWORD>@brd.superproxy.io:9222';

async function main() {
    console.log('Connecting to Scraping Browser...');
    const browser = await puppeteer.connect({ browserWSEndpoint: SBR_WS_ENDPOINT });
    try {
        const page = await browser.newPage();
        console.log('Connected! Navigating to example.com...');
        await page.goto('https://example.com');
        console.log('Scraping page content...');
        const html = await page.content();
        console.log(html);
    } finally {
        await browser.close();
    }
}

main().catch(err => console.error(err.stack || err));
```

> **💡 [Puppeteer로 Webスクレイピング](https://brightdata.co.kr/blog/how-tos/web-scraping-puppeteer)에 대해 더 알아보십시오**

### Playwright 예시 (Python)

```python
import asyncio
from playwright.async_api import async_playwright

SBR_WS_CDP = 'wss://brd-customer-<YOUR-USERNAME>-zone-<YOUR-ZONE-NAME>:<YOUR-PASSWORD>@brd.superproxy.io:9222'

async def run(pw):
    print('Connecting to Scraping Browser...')
    browser = await pw.chromium.connect_over_cdp(SBR_WS_CDP)
    try:
        page = await browser.new_page()
        print('Connected! Navigating to example.com...')
        await page.goto('https://example.com')
        print('Scraping page content...')
        html = await page.content()
        print(html)
    finally:
        await browser.close()

async def main():
    async with async_playwright() as playwright:
        await run(playwright)

if __name__ == '__main__':
    asyncio.run(main())
```

> **💡 [Playwright로 Webスクレイピング](https://brightdata.co.kr/blog/how-tos/playwright-web-scraping)에 대해 더 알아보십시오**

### Selenium 예시 (JavaScript)

```js
const { Builder, Browser } = require('selenium-webdriver');

const SBR_WEBDRIVER = 'https://brd-customer-<YOUR-USERNAME>-zone-<YOUR-ZONE-NAME>:<YOUR-PASSWORD>@brd.superproxy.io:9515';

async function main() {
    console.log('Connecting to Scraping Browser...');
    const driver = await new Builder().forBrowser(Browser.CHROME).usingServer(SBR_WEBDRIVER).build();
    try {
        console.log('Connected! Navigating to example.com...');
        await driver.get('https://example.com');
        console.log('Scraping page content...');
        const html = await driver.getPageSource();
        console.log(html);
    } finally {
        driver.quit();
    }
}

main().catch(err => console.error(err.stack || err));
```

> **💡 [Selenium으로 Webスクレイピング](https://brightdata.co.kr/blog/how-tos/using-selenium-for-web-scraping)에 대해 더 알아보십시오**

## 🔥 고급 기능

### Chrome DevTools로 디버깅

브라우저 セッション을 실시간으로 모니터링합니다:

```js
const { exec } = require('child_process');

const chromeExecutable = 'google-chrome';
const openDevtools = async (page, client) => {
    const frameId = page.mainFrame()._id;
    const { url: inspectUrl } = await client.send('Page.inspect', { frameId });
    exec(`"${chromeExecutable}" "${inspectUrl}"`, error => {
        if (error) throw new Error('Unable to open devtools: ' + error);
    });
};
```

### CAPTCHA 해결

```js
const client = await page.target().createCDPSession();
const { status } = await client.send('Captcha.solve', { detectTimeout: 30000 });
```

> **🤖 [CAPTCHA Solver](https://github.com/luminati-io/Captcha-solver)에 대해 더 알아보십시오.**

## 🔄 자동 IPアドレス ローテーション 및 언락  
Scraping Browser는 통합된 [ローテーティングプロキシ](https://brightdata.co.kr/solutions/rotating-proxies) 덕분에 IPアドレス를 자동으로 로테이션하며, 원활한 데이터 수집을 위해 リトライ도 처리합니다. 

## 💰 가격  

### 유연한 플랜  
- **종량제(Pay-As-You-Go):** $8.40/GB – 약정 없음.  
- **성장 플랜(Growth Plan):** $7.14/GB – 팀에 이상적입니다.  
- **비즈니스 플랜(Business Plan):** $6.30/GB – 운영 확장에 적합합니다.  
- **엔터프라이즈(Enterprise):** 대용량 요구 사항을 위한 맞춤형 가격.  

**지금 가입하고 첫 예치금에 대해 최대 $500까지 매칭 혜택을 받으십시오!**  

[가격 보기](https://brightdata.co.kr/pricing/scraping-browser)  

## ❓ 자주 묻는 질문  

### Scraping Browser는 표준 헤드리스 브라우저와 무엇이 다릅니까?  
Scraping Browser는 Bright Data의 인프라에서 실행되는 완전 관리형 GUI 기반 브라우저로, 가장 강력하게 보호된 사이트까지도 자동으로 언락합니다.  

### Scraping Browser는 봇 탐지를 어떻게 처리합니까?  
ブラウザフィンガープリント, CAPTCHA 해결, リトライ를 자동화하고 실제 사용자 행동을 모방하여 탐지를 방지합니다.  

### Scraping Browser는 Puppeteer, Playwright, Selenium과 호환됩니까?  
네! 모든 주요 웹 자동화 도구와 원활하게 통합됩니다.  

### 언제 プロキシ 대신 Scraping Browser를 사용해야 합니까?  
JavaScript 렌더링, 상호작용 동작(클릭, 스크롤), 다단계 내비게이션이 필요할 때 Scraping Browser를 사용하십시오.