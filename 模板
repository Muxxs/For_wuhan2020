#coding=utf-8

from selenium import webdriver
import requests,json
Province=""
City=""
X_path_title="" #写各自的xpath 从chrome里F12就能找到 右键粘贴过来
X_path_date=""
X_path_content=""

X_path_urllist=""

#这里把要需要遍历的数字改成 #id
#如->  /html/body/div/div/table/tbody/tr[#id]

Key_word=""
#在这里写需要留下来的文章的标题里的关键字 如->肺炎疫情情况


def write_cvs(context,data):
    for key in data:
        context = context + data[key] + ','
    context = context[:-1]
    file = open('testdata1.csv', 'a')
    file.writelines("\n" + context)
    return context


def get_content(url):
    from selenium import webdriver
    from selenium.webdriver.chrome.options import Options
    option = webdriver.ChromeOptions()
    option.add_argument('headless')
    driver = webdriver.Chrome('chromedriver', options=option)
    driver.get(url)
    title=driver.find_element_by_xpath('body > div > div > table > tbody > tr > td > table:nth-child(2) > tbody > tr:nth-child(1) > td > div').text
    publish_date=driver.find_element_by_xpath('/html/body/div/div/table/tbody/tr/td/table[2]/tbody/tr[2]/td/div').text
    content=driver.find_element_by_xpath('/html/body/div/div/table/tbody/tr/td/table[3]/tbody/tr/td').text
    data = {
        'province': Province,
        'city': City,
        'publish_time': publish_date.split(':')[1].split(' ')[1],   #注意要修改一下具体的分隔符和split后的选择
        'publish_date': publish_date.split(':')[1].split(' ')[0],   #注意要修改一下具体的分隔符和split后的选择
        'title': title,
        'content': content,
        'link': url,
        'links_to_pic': '',
        'announce_type': "0",
        'uploader':'Muxxs',  #注意写自己的名字
        'key':'12345'
    }
    #write_cvs("",data)
    #print(data)   #先测试print 再写到数据库
    headers = {'Content-Type': 'application/json'}
    rep = requests.post(headers=headers, data=json.dumps(data), url="http://152.136.160.189/api/add")  #有更改注意文件
    print(rep.text)

    driver.close()
def get_url(url):
    url='http://wsjkw.hebei.gov.cn/index.do?cid=45&templet=gzdt_list&page='
    start=1
    end=5
    url_list=[]
    for i in range(start,end):

        from selenium.webdriver.chrome.options import Options
        import time

        option = webdriver.ChromeOptions()
        option.add_argument('headless')
        driver = webdriver.Chrome('chromedriver', options=option)
        driver.get(url+str(i))
        for id in range(1,21):
            xpath=X_path_urllist.replace("#id",str(id))
            element=driver.find_element_by_xpath(xpath)
            if element.text.find(Key_word)!=-1:
                url_list.append(element.get_attribute('href'))
    for x in url_list:
        get_content(x)
get_url()
