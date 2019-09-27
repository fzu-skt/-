#!/usr/bin/env python
# coding: utf-8

# In[1]:


import re
import cpca
import jieba
import json
import string
import pandas as pd
import numpy as np


# In[2]:


def get_type(s):
    str1=s[:s.find("!")]
    s=s[s.find("!")+1:]
    ty=re.findall(r"\d",str1)
    ty=ty[0]
    #print(s)
   # print(ty)
    return s,ty
#a="[1]!fergjtegjtr4545"
#get_type(a)


# In[3]:


def get_name(s):
    name=s[:s.find(",")]
    str1=s[s.find(",")+1:]
    #print(name)
    #print(str1)
    return str1,name
#a="fergjt,egjtr4545"
#get_name(a)


# In[4]:


def get_tel(s):
    tel=re.findall(r"[0-9]{11}",s)
    str3=tel[0]
    #print(str3)
    #print(s.find(str3))
    str1=s[:s.find(str3)]
    str2=s[s.find(str3)+11:]
    s=str1+str2
    #print(str1)
    #print(str2)
    #print(s)
    return s,str3


# In[5]:


def fir_3(s):
    a=[]
    a.append(s)
    da=cpca.transform(a, umap={} )
    address = np.array(da)
   # str1=np.str(da)
    #str1=str1[str1.find('0')+1:]
   # str1=str1.replace(' ','')
    address = address[0]
    str1=address[3]
    address=address[:3]
    ans=[]
    ans.append(address[0])
    ans.append(address[1])
    ans.append(address[2])
    #print(str1)
    #print(ans)
    return str1,ans
#s="龙海市海澄镇"
#print(fir_3(s))


# In[18]:


def _auto_geo_completion(_address):
    import requests
    _my_key = '02bfc9bce6b516fbf370e753cd56216f'
    
    _parameters_1 = { 'key':_my_key, 'address':_address }
    _url_1 = 'https://restapi.amap.com/v3/geocode/geo?parameters'
    _ret_1 = requests.get(_url_1, _parameters_1).json()
    
    _location = _ret_1['geocodes'][0]['location']
    _parameters_2 = { 'key':_my_key, 'location':_location }
    _url_2 = 'https://restapi.amap.com/v3/geocode/regeo?parameters'
    _ret_2 = requests.get(_url_2, _parameters_2).json()
    
    return _ret_2['regeocode']['formatted_address']
#s=input()
#print(_auto_geo_completion(s))


# In[7]:


def lev_5(s):
    a= ['镇','乡','街道']
    res=""
    ans=""
    for i in a:
        if(s.find(i)>=0):
            ans=s[:s.find(i)+len(i)]
            res=s[s.find(i)+len(i):]
            break
        else:
            res=s
   # print(ans)
    #print(res)
    addr=[]
    addr.append(ans)
    addr.append(res)
    return addr
#str1="海澄镇合浦村高井3号"
#print(lev_5(str1))


# In[8]:


def lev_7(s):
    a = ['路','街','巷','村']
    res=""
    ans=""
    ss=""
    for i in a:
        if(s.find(i)>=0):
            ans=s[:s.find(i)+1]
            res=s[s.find(i)+1:]
            break
        else:
            res=s
    if(res.find('号')>=0):
        ss=res[:res.find('号')+1]
        res=res[res.find('号')+1:]
    
    
    addr=[]
    addr.append(ans)
    addr.append(ss)
    addr.append(res)
    return addr
#str1="海澄镇合浦村高井3号"
#print(lev_7(str1))


# In[24]:


def lev_auto(s):
    ans=[]
    tmp=lev_5(s)
    ans.append(tmp[0])
    aa=lev_7(tmp[1])
    ans.append(aa[0])
    ans.append(aa[1])
    ans.append(aa[2])
    return ans
#str1="海澄镇合浦村高井3号"
#print(lev_auto(str1))


# In[28]:


def main():
    s=input()
    if(s.find('.')>=0):
        s=s[:s.find('.')]
    s,ty=get_type(s)
    s,name=get_name(s)
    s,tel=get_tel(s)
    ans={'姓名':name,'手机':tel}
    
    #print("前三级是：",addr)
    if ty=='1':
        s,addr=fir_3(s)
        _addr=lev_5(s)
        #print("do 1")
    elif ty=='2':
        s,addr=fir_3(s)
        aaa=lev_5(s)
       # print("do 2")
        s=aaa[1]
        tmp_addr=lev_7(s)
        _addr=[]
        sd=aaa[0]
        _addr.append(sd)
        _addr.extend(tmp_addr)
    else:
        g=_auto_geo_completion(s)
        g,addr=fir_3(g)
        s,f=fir_3(s)
        _addr=lev_auto(s)
       # print("do 3")
    addr.extend(_addr)
    
    #print(addr)
    ans['地址']=addr
    import json
    jsonarray =json.dumps(ans, ensure_ascii=False)
    print(jsonarray)  


# In[29]:


main()


# In[ ]:




