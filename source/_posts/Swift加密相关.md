---
title: Swift加密相关
date: 2018-01-08 21:33:01
tags: AES, Base64, MD5
---


最近公司项目开发中引用了 `Swift` 与 `OC` 混编, 涉及到了加密相关, 使用了第三方 `CryptoSwift`, 以下是使用过程中的简单记录

### CryptoSwift简介

- github: <https://github.com/krzyzanowskim/CryptoSwift>
- 使用Swift 编写的加密工具包，支持多种加密算法，如：MD5、SHA1、AES-128, AES-192, AES-256... 等等

### 数组转换

- `data` 转化为字节数组

	```
	let bytes = data.bytes
    print(bytes)
	```

-  字节数组转化为 `data`

	```
	let data = Data(bytes: [0x01, 0x02, 0x03])
    print(data)
	```
- 十六进制创建字节
        
        let bytes_Hex = Array<UInt8>(hex: "0x010203")
        print(bytes_Hex)
        
- 字节转十六进制数

        let hex = bytes_Hex.toHexString()
        print(hex)       
        
        
- 字符串生成字节数组

        let string2bytes = "string".bytes
        print(string2bytes)
        
        
- 字节数组 base64

        let bytes1: [UInt8] = [1, 2, 3]
        let base64Str = bytes1.toBase64()
        print(base64Str!)
        
- 字符串base64

        let str0 = "test"
        let base64Str1 = str0.bytes.toBase64()
        print(base64Str1!)
             

### MD5加密   

- MD5（RFC1321）诞生于 1991 年，全称是“Message-Digest Algorithm(信息摘要算法)5”，由 MIT 的计算机安全实验室和 RSA 安全公司共同提出。
- 之前已经有 MD2、MD3 和 MD4 几种算法。MD5 克服了 MD4 的缺陷，生成 128bit 的摘要信息串，出现之后迅速成为主流算法 
    
- 字节数组MD5

        let str = "测试test"
        let bytes = str.bytes
        
        // 写法一
        let digest = bytes.md5().toHexString()
        print(digest)
        // 04f47b69e3573867a5b3c1e5edb00789

        // 写法二
        let digest1 = Digest.md5(bytes).toHexString()
        print(digest1)
        // 04f47b69e3573867a5b3c1e5edb00789
        
- Data的MD5值
        
        /// data MD5
        let data = Data(str.bytes)
        let digest3 = data.md5().toHexString()
        print(digest3)
        // 04f47b69e3573867a5b3c1e5edb00789
        
- 字符串MD5
        
        ///写法 一
        let digest4 = str.md5()
        print(digest4)
        // 04f47b69e3573867a5b3c1e5edb00789

        // 写法二
        do {
            var digest4_2 = MD5()
            
            let _ = try digest4_2.update(withBytes: "测试".bytes)
            let _ = try digest4_2.update(withBytes: "test".bytes)
            let result = try digest4_2.finish()
            print(result.toHexString())
            //04f47b69e3573867a5b3c1e5edb00789
        } catch {
            
        }    
  
### 哈希算法 
   
- 字节数组SHA值

        let str = "测试test"
        let bytes = str.bytes
        
        //方式一
        let digest1 = bytes.sha1().toHexString()
        let digest2 = bytes.sha224().toHexString()
        let digest3 = bytes.sha256().toHexString()
        let digest4 = bytes.sha384().toHexString()
        let digest5 = bytes.sha512().toHexString()
        print(digest1, digest2, digest3, digest4 ,digest5, separator: "\n")
        
        //方式二
        let digest6 = Digest.sha1(bytes).toHexString()
        let digest7 = Digest.sha224(bytes).toHexString()
        let digest8 = Digest.sha256(bytes).toHexString()
        let digest9 = Digest.sha384(bytes).toHexString()
        let digest10 = Digest.sha512(bytes).toHexString()
        print(digest6, digest7, digest8, digest9 ,digest10, separator: "\n")

                
- Data的SHA值

        let data = Data(bytes: str.bytes)
        let digest11 = data.sha1().toHexString()
        let digest12 = data.sha224().toHexString()
        let digest13 = data.sha256().toHexString()
        let digest14 = data.sha384().toHexString()
        let digest15 = data.sha512().toHexString()
        print(digest11, digest12, digest13, digest14 ,digest15, separator: "\n")

        
        
- 字符串的SHA值
        
        let digest16 = str.sha1()
        let digest17 = str.sha224()
        let digest18 = str.sha256()
        let digest19 = str.sha384()
        let digest20 = str.sha512()
        print(digest16, digest17, digest18, digest19 ,digest20, separator: "\n")
        
        
        // 方式一
        let digest_str = str.sha1()
        print(digest_str)
        
        // 方式二
        do {
            var digest_str = SHA1()
            let _ = try digest_str.update(withBytes: Array("测试".bytes))
            let _ = try digest_str.update(withBytes: Array("test".bytes))
            let result = try digest_str.finish()
            print(result.toHexString())
        } catch {
            
        }
 
### AES 加密

- key：公共密钥

- IV： 密钥偏移量

- padding：key 的补码方式

		key 长度不够16 字节时，需要手动填充, zeroPadding 将其补齐至 blockSize 的整数倍
		zeroPadding 补齐规则:
		- 将长度补齐至 blockSize 参数的整数倍。比如我们将 blockSize 设置为 AES.blockSize（16）
		- 如果长度小于 16 字节：则尾部补 0，直到满足 16 字节。
		- 如果长度大于等于 16 字节，小于 32 字节：则尾部补 0，直到满足 32 字节。
		- 如果长度大于等于 32 字节，小于 48 字节：则尾部补 0，直到满足 48 字节。 以此类推......
            
- 补码方式padding, noPadding, zeroPadding, pkcs7, pkcs5, 默认使用 pkcs7,两者写法结果相同


            let iv_paddding = "1234567890123456" // 默认是
            let aes_padding1 = try AES(key: key.bytes, blockMode: .CBC(iv: iv_paddding.bytes))
            let aes_padding2 = try AES(key: key.bytes, blockMode: .CBC(iv: iv_paddding.bytes), padding: .pkcs7)            
 
            
- ECB 模式

		let key = "hangge.com123456"
		let str = "E蜂通信"
		print("加密前 \(str)")
       
		//MARK: 使用ECB 模式， , 使用aes128 加密
		let aes = try AES(key: key.bytes, blockMode: .ECB)
		let encrpyted = try aes.encrypt(str.bytes)
		print("加密后 \(encrpyted)", separator: "-----")
		
		 /// 解密
		 let decrypted = try aes.decrypt(encrpyted)
		 print("解密后 \(decrypted)")
		 let decryptedString = String(data: Data(decrypted), encoding: .utf8)!
		 print("解密后字符串 \(decryptedString)")
		 
		 
- CBC 模式

		//MARK: 使用CBC 模式, 需要提供一个额外的密钥偏移量 iv
            
            /// 写法 1 --  便捷写法
            // 偏移量
            let iv = "1234567890123456" // 默认是
            
            // TODO: 随机密钥偏移量
            let data = Data(bytes: AES.randomIV(AES.blockSize))
            let random_iv = String(data: data, encoding: .utf8)
            
            
            
            /// 写法 2 --
            // 创建密码器
            let cbc_aes = try AES(key: key.bytes, blockMode: .CBC(iv: iv.bytes))

            let cbc_aes2 = try AES(key: key, iv: iv)
            
            
            let cbc_aes3 = try AES(key: key, iv: iv)
            
            let cbc_encrypted = try cbc_aes.encrypt(str.bytes)
            
            let cbc_encrypted2 = try cbc_aes2.encrypt(str.bytes)
            
            let cbc_encrypted3 = try cbc_aes3.encrypt(str.bytes)
            
            
            // 便捷写法解密
            let cbc_decrypted = try cbc_aes.decrypt(cbc_encrypted)
            let cbc_decryptedData = Data(cbc_decrypted)
            let cbc_decryptedStr = String(data: cbc_decryptedData, encoding: .utf8)
            print(cbc_decryptedStr)
            
            // 正常写法2 解密
            let cbc_decrypted2 = try cbc_aes2.decrypt(cbc_encrypted2)
            let cbc_decryptedStr2 = String(data: Data(cbc_decrypted2), encoding: .utf8)
            print(cbc_decryptedStr2)
            
            
            // 随机密钥偏移量解密
            let cbc_decrypted3 = try cbc_aes3.decrypt(cbc_encrypted3)
            let cbc_decryptedStr3 = String(data: Data(cbc_decrypted3), encoding: .utf8)
            print(cbc_decryptedStr3)

	