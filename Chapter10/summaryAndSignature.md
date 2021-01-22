# 一、摘要

 **消息通过消息摘要算法（特定的哈希函数）得到定长的一串哈希值**

### 1简介：

首先不要局限于行业。名词一样 定有共通处。A行业有XX名词，B行业也有XX名词，真巧。
其实不然，先生活中有对应的概念，然后起名词，再各个行业用到此概念用此名词及派生词（加了行业特点性质）。

* 小时候写作文 。

   xxxxxxxxxxxx （名人名言 ） -----鲁迅。

​       xxxxxxxxxxxxx （文章中精彩的部分） --------摘要。

* 文献，论文，发明专利

  **发明:** *摘要是发明或者实用新型说明书的简明文摘。摘要应简明扼要地叙述发明或者实用新型的主题和实质内		容。摘要的重要目的是便于人们进行文献检索和初步分类*。



​	   **论文：** *论文摘要是对论文的内容不加注释和评论的简短陈述，要求扼要地说明研究工作的目的、研究方法和						最终结论等，重点是结论，是一篇具有独立性和完整性的短文，根据内容的不同，摘要可分为以下						三大类：报道性摘要、指示性摘要和报道指示性摘要。*





​	 **特点：**是普遍很**短**（对于主体），能**标识概况**出主体。



### 2消息摘要算法 Message-Digest Algorithm



消息摘要算法的主要特征是[加密](https://baike.baidu.com/item/加密/752748)过程不需要[密钥](https://baike.baidu.com/item/密钥/101144)，并且经过加密的数据无法被解密，目前可以被解密逆向的只有[CRC32](https://baike.baidu.com/item/CRC32/7460858)算法(CRC:循环冗余校验)，只有输入相同的明文数据经过相同的消息摘要算法才能得到相同的密文<百度百科>。

**特点：** 计算出的结果是定长，不可逆，几乎不碰撞

**消息摘要：**    消息通过消息摘要算法（特定的哈希函数）得到定长的一串哈希值，

信息是任意长度，而摘要是定长。



例子：**md5、sha1、sha256、HMAC**



### 3 api中的应用: 

###   摘要认证

* 注： 此文讲的不是 http摘要认证

* 目的:  是确保数据在网络传输过程中不被篡改



* 发送方：参数排序->参数串起来+key->通过摘要算法（MD5等）生成摘要->传给服务端（此时能区分出参数串和摘要）



* 接收方：获取参数集合和摘要->参数集合排序成串+key->通过摘要算法（MD5等）生成摘要->接收方生成的摘要串和发送方发送的摘要串比对->是否篡改



双方都必须持有key。安全性依赖于key。不够安全，如下列情况

* 接收方人员通过聊天工具把key给发送方人员）

* key比较简单，暴力破解



### 4 摘要和加密易混点

例子：

**错误的说法 : **  md5加密

md5算法是散列算法，不具备加解密功能，一旦md5计算摘要，不能通过算法反向还原成原文。

凡是说md5加解密一律错误。根据摘要得到原文是根据彩虹表来的。



加密算法：

**对称性加密算法有：AES、DES、3DES**

**非对称性算法有：RSA、DSA、ECC**

   

#  二、 签名

**定义**： **对摘要进行加密后的结果**



### 签名认证。

与摘要认证大致相同，区别在用于把摘要进行加密（通过加密算法）。接收方先解密摘要，再验证。









### 哈希算法，摘要算法，**散列算法**  //TODO 

摘要算法是特殊的哈希算法。

表现在：计算后（不能说加密后），定长。不可逆等。





**常见错误：**


