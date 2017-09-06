# wordcloud
word使用
# 日常存数据进入mongodb
import jieba
from scipy.misc import imread
from wordcloud import WordCloud, STOPWORDS, ImageColorGenerator
import matplotlib.pyplot as plt
from PIL import Image
from os import path
import numpy
from selenium import webdriver
import time
import pymongo

client = pymongo.MongoClient('localhost', 27017)
douban = client['douban']
zhanlang = douban['zhanlang']

d = path.dirname(__file__)
chromedriver = 'C:/Users/Administrator/AppData/Local/Google/Chrome/Application/chromedriver.exe'
dr = webdriver.Chrome(chromedriver)
def get_all_comments():
    login_url = 'https://www.douban.com/accounts/login?source=main'
    user_name = '18617126861'
    password = 'Panny2601039'
    chromedriver = 'C:/Users/Administrator/AppData/Local/Google/Chrome/Application/chromedriver.exe'
    dr = webdriver.Chrome(chromedriver)
    dr.get(login_url)
    dr.find_element_by_id('email').clear()
    dr.find_element_by_id('email').send_keys(user_name)
    dr.find_element_by_id('password').clear()
    dr.find_element_by_id('password').send_keys(password)
    captcha_field = input('输入验证码:')
    dr.find_element_by_id('captcha_field').send_keys(captcha_field)
    dr.find_element_by_class_name('btn-submit').click()
    time.sleep(4)
    movie_url = 'https://movie.douban.com/subject/26363254/comments?status=P'
    dr.get(movie_url)
    content_list = []
    n = 0
    while True:
        try:
            results = dr.find_elements_by_class_name('comment')
            for each_comment in results:
                info = {
                    # 获取ID
                    'author': each_comment.find_elements_by_tag_name('a')[1].text,
                    # 获取二级信息不能为list
                    'vote': each_comment.find_element_by_class_name('comment-vote').find_element_by_class_name('votes').text,
                    # 获取内容
                    'content': each_comment.find_element_by_tag_name('p').text
                }
                zhanlang.insert(info)
                content = each_comment.find_element_by_tag_name('p').text
                content_list.append(content + '\n')
            dr.find_element_by_class_name('next').click()
            n = n + 1
            print('第{}页查找完毕'.format
                  (n))
            time.sleep(4)
        except Exception as e:
            print(str(e))
        with open('F:/战狼.txt', 'w', encoding='utf-8') as f:
            for each_one in content_list:
                f.write(each_one)
def info_operation():
    word_list = []
    with open(path.join(d, '战狼.txt'), 'r', encoding='utf-8') as f:
        raw_infos = f.readlines()
        for raw_info in raw_infos:
            cut_list = list(jieba.cut(raw_info))
            for word in cut_list:
                word_list.append(word)
    with open(path.join(d, 'words_cut.txt'), 'w', encoding='utf-8') as zl:
        for each_one in word_list:
            zl.write(each_one + '\n')
    return zl

def word_cloud():
    text = open(path.join(d, 'words_cut.txt'), 'r', encoding='utf-8').read()
    mask = numpy.array(Image.open(path.join(d, 'ceshi.jpg')))
    word_lists = jieba.cut(text, cut_all=True)
    word_list = ''.join(word_lists)

    word_cloud = WordCloud(background_color='white',
                           mask=mask,
                           max_words=2000,
                           font_path='C:/Users/Administrator/Desktop/方正报宋简体.ttf',
                           random_state=30)
    rs = word_cloud.generate(word_list)
    word_cloud.to_file(path.join(d, '3.jpg'))

    plt.imshow(rs)
    plt.axis("off")
    plt.figure()
    plt.imshow(mask, cmap=plt.gray())
    plt.axis("off")
    plt.show()


if __name__ == '__main__':
    # get_all_comments()
    info_operation()
    word_cloud()
