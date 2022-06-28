###### [Home](https://eddiegranados.github.io/Eduardo_Granados/)

---

- I have been wanting to purchase an Xbox Series X for a long time, and I have always been beat by the bots out there.
So I thought the only way to even have a chance at purchasing one was by fighting fire with fire.
Below is my way of purchasing items with the use of Playwright, a Python Library

--- 

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
    browser = firefox.launch()
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
            # Adding item to cart
            page.wait_for_selector('.c-button-primary')
            page.click('.c-button-primary')
            print("Adding to cart...")

            # Going to my cart 
            page.goto('https://www.bestbuy.com/cart')
            print("Going to cart...")

            # Checking out item
            page.wait_for_selector('.btn-lg')
            page.click('.btn-lg')
            print("Checking out in...")

            # entering email
            page.wait_for_selector('#fld-e')
            page.fill('#fld-e', config.email)
            print("Entering email...")
            
            # entering password
            page.wait_for_selector('#fld-p1')
            page.fill('#fld-p1', config.pw)
            print("Entering password...")
            
            # Log in to account
            # page.click('.cia-form__controls__submit')
            # print("Logging in")          
            page.wait_for_timeout(3000)
            print("complete")
            browser.close()
            buyButton = True

print(f'Program took {time.time()-start_time:.3f} seconds to run')
```