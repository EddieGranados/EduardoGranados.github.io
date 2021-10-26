```bash
import time # importing the time library
start_time=time.time() # setting the variable start_time = equal to the start
import config # import config file

# if playwright is installed, Playwright can be imported in a Python script
# and launch any of the 3 browsers (chromium, firefox and webkit).
from playwright.sync_api import sync_playwright

buyButton = False

with sync_playwright() as p:
    firefox = p.firefox
    browser = firefox.launch(headless=False)
    page = browser.new_page()
    page.goto('https://www.bestbuy.com/site/microsoft-controller-for-xbox-series-x-xbox-series-s-and-xbox-one-latest-model-carbon-black/6430655.p?skuId=6430655')

    # loop continuously until item has been bought
    while not buyButton:
        try:
            # if the button to buy is disabled, refresh page until its enabled
            if page.is_disabled('.c-button-disabled') == True:
                print("Button isn't ready yet")
                print("Refreshing...")
                page.reload()
                print(f'{time.time()-start_time:.3f} seconds have passed')
            else:
                continue
        except:
            page.click('.c-button-primary')
            print("Adding to cart...")
            page.wait_for_timeout(3000)
            page.goto('https://www.bestbuy.com/cart')
            print("Going to cart...")
            page.wait_for_timeout(3000)
            page.click('.btn-lg')
            print("Checking out in...")
            page.wait_for_timeout(3000)
            page.fill('#fld-e',config.email)
            print("Entering email...")
            page.fill('#fld-p1', config.pw)
            print("Entering password...")
            # page.click('.cia-form__controls__submit')
            # print("Logging in")          
            page.screenshot(path="example.png")
            page.wait_for_timeout(3000)
            print("complete")
            browser.close()
            buyButton = True

print(f'Program took {time.time()-start_time:.3f} seconds to run')
```

---

###### [Home](https://eddiegranados.github.io/Eduardo_Granados/)