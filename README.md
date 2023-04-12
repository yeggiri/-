# -
#Selenium을 활용하여 구글, 네이버에서 이미지 스크랩
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By
import time
import urllib.request
import ssl

ssl._create_default_https_context = ssl._create_unverified_context

driver = webdriver.Chrome()
driver.get("https://www.google.co.kr/imghp?hl=en&ogbl") #구글 이미지창 열기
lem = driver.find_element(By.NAME, "q") #검색창 찾기
lem.send_keys("elevator door") #검색하고 싶은 이름 작성
lem.send_keys(Keys.RETURN) #엔터키

last_height = driver.execute_script("return document.body.scrollHeight")
while True:  
    driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
  
    time.sleep(1.5)
    
    new_height = driver.execute_script("return document.body.scrollHeight")
    if new_height == last_height:
        try:
            driver.find_element(By.CSS_SELECTOR, ".mye4qd").click()
        except:
            break
    last_height = new_height #스크롤 쭉 내리기

time.sleep(2)
images = driver.find_elements(By.CSS_SELECTOR, ".rg_i.Q4LuWd") #작은 이미지 선택

count = 1
for image in images:
    try:
        image.click() #작은 이미지 클릭
        time.sleep(3)
        Url = driver.find_element(By.CSS_SELECTOR, ".r48jcc.pT0Scc.iPVvYb").get_attribute("src") #큰 이미지 url 얻기
        #Url = driver.find_element(By.XPATH, "/html/body/div[2]/c-wiz/div[3]/div[2]/div[3]/div[2]/div/div[2]/div[2]/div[2]/c-wiz/div/div[2]/div[1]/a/img[1]").get_attribute("src")
#Url 위에 2개 중 하나 택
        imgURL = Url
        urllib.request.urlretrieve(imgURL, "elevatorDoor " + str(count) +".jpg") #얻은 큰 이미지 다운로드
        count += 1
    except Exception as e:
        print(e)
        continue
    
time.sleep(5)
driver.quit()

# assert "Python" in driver.title
# elem = driver.find_element(By.NAME, "q")
# elem.clear()
# elem.send_keys("pycon")
# elem.send_keys(Keys.RETURN)
# assert "No results found." not in driver.page_source
# driver.close()

#파이썬 가상환경 만드는 코드 : python -m venv "이름"
