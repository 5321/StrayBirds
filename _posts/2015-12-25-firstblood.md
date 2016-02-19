---
layout: default
title: First Blood
comments: false
category: 技术
---


# FIRST BLOOD

程序员电脑上一般都会有很多电子书籍，有的书名字都差不多，时间长了也不记得哪些书读过，哪些书没读过。
初学Python，用它写了个小程序，生成bookList.txt文件来记录书籍是否读过：
1、将check_books.py文件放在存放电子书籍的目录下；
2、运行check_books.py -create命令初次生成记录文件bookList.txt；
3、打开bookList.txt，将读过的书籍在书籍名前面的括号中标记“*”号（注意（）中的空格，标记*号后没有空格）；
4、该文件夹下有新增书籍时运行check_books.py会自动在记录文件中增加书籍，并将标记置为未读；
5、通过check_books.py “文件名”可查询该书籍是否已读；
6、打开bookList.txt文件浏览查看书籍是否已读；
7、同样可用于电影、电视剧记录，方法一样。
>代码：
>>[python] view plain copy
#-*-coding:utf-8 -*-  
##################################################################################  
#file name:check_books.py  
#功能：记录当前目录book是否已读，已读标记(*)，未读标记( )  
#版本：v0.0  
#第一次生成记录文件：check_books.py -create  
#目录新增文件是更新记录文件：check_books.py  
#查看某一书籍是否已读：check_books.py 文件名  
##################################################################################  
#待更新:文件夹迭代查询记录  
##################################################################################  
import os,sys  
  
def main():  
    FILE_NAME = 'bookList.txt'  
    book_mark_dict = {}  
  
    pwd = os.path.dirname(__file__)  
    book_list = os.listdir(pwd)  
    MARK = "(*)"  
    NOTMARK = "( )"  
  
    try:  
        is_create = sys.argv[1]  
        #新建book list 文件，所有book初始化为未读  
        if is_create == '-create':  
            print "Create New book list!"  
            create_book_list(FILE_NAME,book_list,NOTMARK)  
            return  
    except IndexError,SyntaxError:    
        print "Updating book list..."  
  
    updateFlag = False  
    book_mark_dict = read_book_list(FILE_NAME)  
    for book in book_list:  
        if book_mark_dict.has_key(book): #book已经存在，不处理  
            pass  
        else: #book不存在，插入字典,初始化为未读  
            print '%s do not exist,update the list...' %(book)  
            updateFlag = True  
            book_mark_dict[book] = NOTMARK  
  
    f = open(FILE_NAME,'w')  
    for book in book_mark_dict:  
        f.write('%s%s%s' %(book_mark_dict[book],book,'\n'))  
    f.close()  
  
    #查询某一book已读标记  
    try:  
        book_name = sys.argv[1]  
        if book_name in book_list:  
            print "Check the book %s read flag!" %(book_name)  
            read_flag = book_mark_dict[book_name]  
            if read_flag == MARK:  
                print "You have read it!"  
            elif read_flag == NOTMARK:  
                print "You have not read it!"  
            else:  
                print "Error!"  
            return   
        else:  
            print 'You input book name error!'  
            return  
    except IndexError,SyntaxError:  
        pass  
  
    if updateFlag == True:  
        print "Update bookList finished!"  
    else:  
        print "No book to update!"  
  
def create_book_list(file_name,book_list,mark):  
    f = open(file_name,'w')  
    for book_name in book_list:  
        f.write('%s%s%s' %(mark,book_name,'\n'))  
    f.close()  
  
#读取book list文件，返回文件名和是否已读标记的键值对  
def read_book_list(file_name):  
    try:  
        f = open(file_name,'r')  
    except IOError:  
        print "BookList do not existed,Please Create bookList First!"  
        print 'Input "check_books.py -create" to create bookList.txt'  
        sys.exit()  
      
    book_dict = {}  
    for eachLine in f.readlines():  
        book_dict[eachLine[3:-1]] = eachLine[0:3]  
    f.close()  
    return book_dict  
  
if __name__ == '__main__':  
    main()  
